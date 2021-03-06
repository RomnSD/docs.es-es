---
title: Protocolo de mensajería de confianza versión 1,1
ms.date: 03/30/2017
ms.assetid: 0da47b82-f8eb-42da-8bfe-e56ce7ba6f59
ms.openlocfilehash: 9320787317131f42c4a82c6114a16fdea87567f4
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2019
ms.locfileid: "74283311"
---
# <a name="reliable-messaging-protocol-version-11"></a>Protocolo de mensajería de confianza versión 1,1

En este tema se tratan los detalles de implementación de Windows Communication Foundation (WCF) para el protocolo WS-ReliableMessaging de febrero 2007 (versión 1,1) necesario para la interoperación mediante el transporte HTTP. WCF sigue la especificación WS-ReliableMessaging con las restricciones y aclaraciones explicadas en este tema. Tenga en cuenta que el protocolo WS-ReliableMessaging versión 1,1 se implementa a partir de .NET Framework 3,5.

El <xref:System.ServiceModel.Channels.ReliableSessionBindingElement>implementa el protocolo WS-ReliableMessaging de febrero de 2007.

Para su comodidad, el tema utiliza las siguientes funciones:

- Iniciador: el cliente que inicia la creación de la secuencia de mensajes WS-Reliable.

- Respondedor: el servicio que recibe las solicitudes del iniciador.

 En este documento, se utilizan los prefijos y espacios de nombres de la tabla siguiente.

|Prefijo|Espacio de nombres|
|-|-|
|wsrm|http://docs.oasis-open.org/ws-rx/wsrm/200702|
|netrm|http://schemas.microsoft.com/ws/2006/05/rm|
|s|http://www.w3.org/2003/05/soap-envelope|
|wsa|http://schemas.xmlsoap.org/ws/2005/08/addressing|
|wsse|http://docs.oasis-open.org/wss/2004/01/oasis-200401-wssecurity-secext-1.0.xsd|
|wsrmp|http://docs.oasis-open.org/ws-rx/wsrmp/200702|
|netrmp|http://schemas.microsoft.com/ws-rx/wsrmp/200702|
|wsp|(Directiva WS-Policy 1.2 o WS-Policy 1.5)|

## <a name="messaging"></a>Mensajería

### <a name="sequence-creation"></a>Creación de secuencias

WCF implementa `CreateSequence` y `CreateSequenceResponse` mensajes para establecer una secuencia de mensajería confiable. Las siguientes restricciones son aplicables:

- B1101: el iniciador de WCF usa la misma referencia de extremo que la `ReplyTo`, `AcksTo` y `Offer/Endpoint`del mensaje de `CreateSequence`.

- R1102: las referencias de punto de conexión `AcksTo`, `ReplyTo` y `Offer/Endpoint` en el mensaje `CreateSequence` deben tener valores de dirección con representaciones de cadena idénticas, de tal modo que coincidan por octetos.

  - El respondedor de WCF comprueba que la parte del URI de las referencias de extremo `AcksTo`, `ReplyTo` y `Endpoint` es idéntica antes de crear una secuencia.

- R1103: las referencias de punto de conexión `AcksTo`, `ReplyTo` y `Offer/Endpoint` en el mensaje `CreateSequence` deberían tener el mismo conjunto de parámetros de referencia.

  - WCF no exige, pero supone que los parámetros de referencia de las referencias de extremo `AcksTo`, `ReplyTo` y `Offer/Endpoint` en `CreateSequence` son idénticos y usan los parámetros de referencia de la referencia de extremo `ReplyTo` para las confirmaciones y los mensajes de secuencia inversa.

- B1104: el iniciador de WCF no genera el elemento `Expires` o `Offer/Expires` opcional en el mensaje de `CreateSequence`.

- B1105: al obtener acceso al mensaje de `CreateSequence`, el respondedor de WCF usa el valor `Expires` en el elemento `CreateSequence` como el valor `Expires` en el elemento `CreateSequenceResponse`. De lo contrario, el respondedor de WCF Lee y omite los valores de `Expires` y `Offer/Expires`.

- B1106: al obtener acceso al mensaje de `CreateSequenceResponse`, el iniciador de WCF lee el valor de `Expires` opcional, pero no lo usa.

- B1107: el iniciador y el respondedor de WCF siempre generan el elemento de `IncompleteSequenceBehavior` opcional en los elementos `CreateSequence/Offer` y `CreateSequenceResponse`.

- B1108: WCF usa solo los valores `DiscardFollowingFirstGap` y `NoDiscard` en el elemento `IncompleteSequenceBehavior`.

  - WS-ReliableMessaging utiliza el mecanismo `Offer` para establecer las dos secuencias correlacionadas inversas que forman una sesión.

- B1109: si `CreateSequence` contiene un elemento `Offer`, el respondedor WCF unidireccional rechaza la secuencia proporcionada respondiendo con una `CreateSequenceResponse` sin un elemento `Accept`.

- B1110: Si un respondedor de mensajería de confianza rechaza la secuencia ofrecida, el iniciador de WCF genera un error en la secuencia recién establecida.

- B1111: si `CreateSequence` no contiene un elemento `Offer`, el respondedor WCF bidireccional rechaza la secuencia proporcionada respondiendo a un error de `CreateSequenceRefused`.

- R1112: cuando dos secuencias inversas se establecen utilizando el mecanismo `Offer`, la propiedad `[address]` de la referencia de punto de conexión `CreateSequenceResponse/Accept/AcksTo` debe coincidir con el URI de destino del mensaje `CreateSequence` byte a byte.

- R1113: cuando dos secuencias inversas se establecen utilizando el mecanismo `Offer`, todos los mensajes en ambas secuencias que fluyen desde el iniciador hasta el respondedor se deben enviar a la misma referencia de punto de conexión.

WCF utiliza WS-ReliableMessaging para establecer sesiones confiables entre el iniciador y el respondedor. La implementación de WCF WS-ReliableMessaging proporciona una sesión confiable para patrones de mensajería dúplex completa y de solicitud-respuesta unidireccionales. El mecanismo `Offer` de WS-ReliableMessaging en `CreateSequence` y `CreateSequenceResponse` le permite establecer dos secuencias inversas correlacionadas y proporciona un protocolo de sesión que es apto para todos los puntos de conexión de mensajes. Dado que WCF proporciona una garantía de seguridad para este tipo de sesión, incluida la protección de un extremo a otro para la integridad de la sesión, es práctico asegurarse de que los mensajes destinados a la misma parte lleguen al mismo destino. Esto también permite el “apoyo a caballo” de confirmaciones de secuencias en mensajes de aplicaciones. Por lo tanto, las restricciones R1102, R1112 y R1113 se aplican a WCF.

Ejemplo de mensaje `CreateSequence`.

```xml
<s:Envelope>
  <s:Header>
    <wsa:Action s:mustUnderstand="1">http://docs.oasis-open.org/ws-rx/wsrm/200702/CreateSequence</wsa:Action>
    <wsa:MessageID>urn:uuid:949cca61-8813-42ff-ab33-18d9e3fa82fa</wsa:MessageID>
    <wsa:ReplyTo>
        <wsa:Address>http://Business456.com/clientA</wsa:Address>
    </wsa:ReplyTo>
    <wsa:To s:mustUnderstand="1">http://BusinessABC.com/serviceA</wsa:To>
  </s:Header>
  <s:Body>
    <wsrm:CreateSequence>
      <wsrm:AcksTo>
        <wsa:Address>http://Business456.com/clientA</wsa:Address>
      </wsrm:AcksTo>
      <wsrm:Offer>
        <wsrm:Identifier>urn:uuid:066b4730-fc82-458a-a5c1-210be4fb4e4e</wsrm:Identifier>
        <wsrm:Endpoint>
          <wsa:Address>http://Business456.com/clientA</wsa:Address>
        </wsrm:Endpoint>
        <wsrm:IncompleteSequenceBehavior>DiscardFollowingFirstGap</wsrm:IncompleteSequenceBehavior>
      </wsrm:Offer>
    </wsrm:CreateSequence>
  </s:Body>
</s:Envelope>
```

Ejemplo de mensaje `CreateSequenceResponse`.

```xml
<s:Envelope>
  <s:Header>
    <wsa:Action s:mustUnderstand="1">http://docs.oasis-open.org/ws-rx/wsrm/200702/CreateSequenceResponse</wsa:Action>
    <wsa:RelatesTo>urn:uuid:949cca61-8813-42ff-ab33-18d9e3fa82fa</wsa:RelatesTo>
    <wsa:To s:mustUnderstand="1">http://Business456.com/clientA</wsa:To>
  </s:Header>
  <s:Body>
    <wsrm:CreateSequenceResponse>
      <wsrm:Identifier>urn:uuid:656652b8-9af2-4e94-9d07-2dc21c05ed27</wsrm:Identifier>
      <wsrm:IncompleteSequenceBehavior>DiscardFollowingFirstGap</wsrm:IncompleteSequenceBehavior>
      <wsrm:Accept>
        <wsrm:AcksTo>
          <wsa:Address>http://BusinessABC.com/serviceA</wsa:Address>
        </wsrm:AcksTo>
      </wsrm:Accept>
    </wsrm:CreateSequenceResponse>
  </s:Body>
</s:Envelope>
```

### <a name="closing-a-sequence"></a>Cierre de una secuencia

WCF usa los mensajes de `CloseSequence` y `CloseSequenceResponse` para un cierre Iniciado por el origen de mensajería de confianza. El destino de la mensajería de confianza de WCF no inicia el cierre y el origen de la mensajería de confianza de WCF no admite el apagado Iniciado por el destino de la mensajería de confianza. Las siguientes restricciones son aplicables:

- B1201: el origen de la mensajería de confianza de WCF siempre envía un mensaje de `CloseSequence` para cerrar la secuencia.

- B1202: el origen de la mensajería de confianza espera la confirmación del intervalo completo de mensajes de secuencia antes de enviar el mensaje `CloseSequence`.

- B1203: el origen de la mensajería de confianza siempre incluye el elemento opcional `LastMsgNumber`, a menos que la secuencia no contenga mensajes.

- R1204: el destino de la mensajería de confianza no debe iniciar el cierre mediante el envío de un mensaje `CloseSequence`.

- B1205: al recibir un mensaje de `CloseSequence`, el origen de mensajería confiable de WCF considera que la secuencia está incompleta y envía un error.

 Ejemplo de mensaje `CloseSequence`.

```xml
<s:Envelope>
  <s:Header>
    <wsa:Action s:mustUnderstand="1">http://docs.oasis-open.org/ws-rx/wsrm/200702/CloseSequence</wsa:Action>
    <wsa:MessageID>urn:uuid:6ce1d4c3-e1c1-474f-a8c9-4210e37f7877</wsa:MessageID>
    <wsa:ReplyTo>
      <wsa:Address>http://Business456.com/clientA</wsa:Address>
    </wsa:ReplyTo>
    <wsa:To s:mustUnderstand="1">http://BusinessABC.com/serviceA</wsa:To>
  </s:Header>
  <s:Body>
    <wsrm:CloseSequence>
      <wsrm:Identifier>urn:uuid:656652b8-9af2-4e94-9d07-2dc21c05ed27</wsrm:Identifier>
      <wsrm:LastMsgNumber>30</wsrm:LastMsgNumber>
    </wsrm:CloseSequence>
  </s:Body>
</s:Envelope>
```

Ejemplo de mensaje de `CloseSequenceResponse`:

```xml
<s:Envelope>
  <s:Header>
    <wsrm:SequenceAcknowledgement>
      <wsrm:Identifier>urn:uuid:656652b8-9af2-4e94-9d07-2dc21c05ed27</wsrm:Identifier>
      <wsrm:AcknowledgementRange Lower="1" Upper="30"></wsrm:AcknowledgementRange>
      <wsrm:Final></wsrm:Final>
      <netrm:BufferRemaining>8</netrm:BufferRemaining>
    </wsrm:SequenceAcknowledgement>
    <wsa:Action s:mustUnderstand="1">http://docs.oasis-open.org/ws-rx/wsrm/200702/CloseSequenceResponse</wsa:Action>
    <wsa:RelatesTo>urn:uuid:6ce1d4c3-e1c1-474f-a8c9-4210e37f7877</wsa:RelatesTo>
    <wsa:To s:mustUnderstand="1">http://Business456.com/clientA</wsa:To>
  </s:Header>
  <s:Body>
    <wsrm:CloseSequenceResponse>
      <wsrm:Identifier>urn:uuid:656652b8-9af2-4e94-9d07-2dc21c05ed27</wsrm:Identifier>
    </wsrm:CloseSequenceResponse>
  </s:Body>
</s:Envelope>
```

### <a name="sequence-termination"></a>Finalización de secuencia

WCF usa principalmente el protocolo de enlace de `TerminateSequence/TerminateSequenceResponse` después de completar el protocolo de enlace de `CloseSequence/CloseSequenceResponse`. El destino de la mensajería de confianza de WCF no inicia la terminación y el origen de la mensajería de confianza no admite una terminación iniciada por el destino de la mensajería de confianza. Las siguientes restricciones son aplicables:

- B1301: el iniciador de WCF solo envía el mensaje de `TerminateSequence` después de la finalización correcta del Protocolo de enlace de `CloseSequence/CloseSequenceResponse`.

- R1302: WCF valida que el elemento `LastMsgNumber` es coherente en todos los mensajes `CloseSequence` y `TerminateSequence` de una secuencia determinada. Esto significa que `LastMsgNumber` no está presente en todos los mensajes `CloseSequence` y `TerminateSequence`, o está presente y es idéntico en todos los mensajes `CloseSequence` y `TerminateSequence`.

- B1303: al recibir un mensaje `TerminateSequence` después del protocolo de enlace `CloseSequence/CloseSequenceResponse`, el destino de la mensajería de confianza responde con un mensaje `TerminateSequenceResponse`. Puesto que el origen de la mensajería de confianza recibe la confirmación definitiva antes de enviar el mensaje `TerminateSequence`, el destino de la mensajería de confianza sabe sin duda que finaliza la secuencia y reclama recursos inmediatamente.

- B1304: al recibir un mensaje de `TerminateSequence` antes del Protocolo de enlace de `CloseSequence/CloseSequenceResponse`, el destino de la mensajería de confianza de WCF responde con un mensaje de `TerminateSequenceResponse`. Si el destino de la mensajería de confianza determina que no hay incoherencias en la secuencia, el destino de la mensajería de confianza espera durante un tiempo especificado por el destino de la aplicación antes de reclamar recursos, para permitir al cliente que reciba la confirmación final. De lo contrario, el destino de la mensajería de confianza reclama recursos inmediatamente e indica al destino de la aplicación que la secuencia finaliza de manera dudosa iniciando el evento `Faulted`.

Ejemplo de mensaje `TerminateSequence`.

```xml
<s:Envelope>
  <s:Header>
    <wsa:Action s:mustUnderstand="1">http://docs.oasis-open.org/ws-rx/wsrm/200702/TerminateSequence</wsa:Action>
    <wsa:MessageID>urn:uuid:3597a398-4f3c-40f4-9335-8f1515572fdf</wsa:MessageID>
    <wsa:ReplyTo>
      <wsa:Address>http://Business456.com/clientA</wsa:Address>
    </wsa:ReplyTo>
    <wsa:To s:mustUnderstand="1">http://BusinessABC.com/serviceA</wsa:To>
  </s:Header>
  <s:Body>
    <wsrm:TerminateSequence>
      <wsrm:Identifier>urn:uuid:656652b8-9af2-4e94-9d07-2dc21c05ed27</wsrm:Identifier>
      <wsrm:LastMsgNumber>30</wsrm:LastMsgNumber>
      </wsrm:TerminateSequence>
  </s:Body>
</s:Envelope>
```

Ejemplo de mensaje de `TerminateSequenceResponse`:

```xml
<s:Envelope>
  <s:Header>
    <wsrm:SequenceAcknowledgement>
      <wsrm:Identifier>urn:uuid:656652b8-9af2-4e94-9d07-2dc21c05ed27</wsrm:Identifier>
      <wsrm:AcknowledgementRange Lower="1" Upper="30"></wsrm:AcknowledgementRange>
      <wsrm:Final></wsrm:Final>
      <netrm:BufferRemaining>8</netrm:BufferRemaining>
    </wsrm:SequenceAcknowledgement>
    <wsa:Action s:mustUnderstand="1">http://docs.oasis-open.org/ws-rx/wsrm/200702/TerminateSequenceResponse</wsa:Action>
    <wsa:RelatesTo>urn:uuid:3597a398-4f3c-40f4-9335-8f1515572fdf</wsa:RelatesTo>
    <wsa:To s:mustUnderstand="1">://Business456.com/clientA</wsa:To>
  </s:Header>
  <s:Body>
    <wsrm:TerminateSequenceResponse>
      <wsrm:Identifier>urn:uuid:656652b8-9af2-4e94-9d07-2dc21c05ed27</wsrm:Identifier>
    </wsrm:TerminateSequenceResponse>
  </s:Body>
</s:Envelope>
```

### <a name="sequences"></a>Secuencias

A continuación, se muestra una lista de restricciones que se aplican a las secuencias:

- B1401: WCF genera y obtiene acceso a los números de secuencia que no son mayores que el valor inclusivo máximo de `xs:long`, 9223372036854775807.

Ejemplo de encabezado `Sequence`.

```xml
<wsrm:Sequence s:mustUnderstand="1">
  <wsrm:Identifier>urn:uuid:656652b8-9af2-4e94-9d07-2dc21c05ed27</wsrm:Identifier>
  <wsrm:MessageNumber>1</wsrm:MessageNumber>
</wsrm:Sequence>
```

### <a name="request-acknowledgement"></a>Solicitar confirmación

WCF usa el encabezado `AckRequested` como un mecanismo Keep-Alive.

Ejemplo de encabezado `AckRequested`.

```xml
<wsrm:AckRequested>
  <wsrm:Identifier>urn:uuid:656652b8-9af2-4e94-9d07-2dc21c05ed27</wsrm:Identifier>
</wsrm:AckRequested>
```

### <a name="sequenceacknowledgement"></a>SequenceAcknowledgement

WCF usa un mecanismo de "reutilizable" para las confirmaciones de secuencias proporcionadas en la mensajería de confianza de WS. Las siguientes restricciones son aplicables:

- R1601: cuando dos secuencias inversas se establecen mediante el mecanismo de `Offer`, el encabezado de `SequenceAcknowledgement` se puede incluir en cualquier mensaje de aplicación que se transmita al destinatario previsto. El extremo remoto debe poder tener acceso a un encabezado `SequenceAcknowledgement` superpuesto.

- B1602: WCF no genera `SequenceAcknowledgement` encabezados que contengan elementos `Nack`. WCF valida que cada elemento de `Nack` contiene un número de secuencia, pero de lo contrario omite el elemento `Nack` y el valor.

 Ejemplo de encabezado `SequenceAcknowledgement`.

```xml
<wsrm:SequenceAcknowledgement>
  <wsrm:Identifier>urn:uuid:656652b8-9af2-4e94-9d07-2dc21c05ed27</wsrm:Identifier>
  <wsrm:AcknowledgementRange Lower="1" Upper="1"></wsrm:AcknowledgementRange>
</wsrm:SequenceAcknowledgement>
```

### <a name="ws-reliablemessaging-faults"></a>Errores de WS-ReliableMessaging

A continuación se muestra una lista de restricciones que se aplican a la implementación de WCF de errores de WS-ReliableMessaging. Las siguientes restricciones son aplicables:

- B1701: WCF no genera errores de `MessageNumberRollover`.

- B1702: sobre SOAP 1,2, cuando el extremo de servicio alcanza su límite de conexiones y no puede procesar nuevas conexiones, WCF genera un subcódigo de error anidado `CreateSequenceRefused`, `netrm:ConnectionLimitReached`, tal y como se muestra en el ejemplo siguiente.

```xml
<s:Envelope>
  <s:Header>
    <wsa:Action>http://docs.oasis-open.org/ws-rx/wsrm/200702/fault</wsa:Action>
  </s:Header>
  <s:Body>
    <s:Fault>
      <s:Code>
        <s:Value>s:Receiver</s:Value>
        <s:Subcode>
          <s:Value>wsrm:CreateSequenceRefused</s:Value>
          <s:Subcode>
            <s:Value>netrm:ConnectionLimitReached</s:Value>
          </s:Subcode>
        </s:Subcode>
      </s:Code>
      <s:Reason>
        <s:Text xml:lang="en">Server 'http://BusinessABC.com/serviceA' is too busy to process this request. Try again later.</s:Text>
      </s:Reason>
    </s:Fault>
  </s:Body>
</s:Envelope>
```

### <a name="ws-addressing-faults"></a>Errores WS-Addressing

Dado que WS-ReliableMessaging usa WS-Addressing, la implementación de WS-ReliableMessaging de WCF puede generar y transmitir errores de WS-Addressing. En esta sección se tratan los errores de WS-Addressing que WCF genera y transmite explícitamente en la capa de WS-ReliableMessaging:

- B1801: WCF genera y transmite el error `Message Addressing Header Required` cuando se cumple una de las siguientes condiciones:

  - A un mensaje `CreateSequence`, `CloseSequence`, o `TerminateSequence` le falta un encabezado `MessageId`.

  - A un mensaje `CreateSequence`, `CloseSequence`, o `TerminateSequence` le falta un encabezado `ReplyTo`.

  - A un mensaje `CreateSequenceResponse`, `CloseSequenceResponse`, o `TerminateSequenceResponse` le falta un encabezado `RelatesTo`.

- B1802: WCF genera y transmite el error de `Endpoint Unavailable` para indicar que no hay ningún extremo escuchando que pueda procesar la secuencia en función del examen de los encabezados de direccionamiento en el mensaje de `CreateSequence`.

## <a name="protocol-composition"></a>Composición de protocolos

### <a name="composition-with-ws-addressing"></a>Composición con WS-Addressing

WCF admite dos versiones de WS-Addressing: WS-Addressing 2004/08 [WS-ADDR] y W3C WS-Addressing 1,0 Recommendations [WS-ADDR-CORE] y [WS-ADDR-SOAP].

Aunque la especificación de WS-ReliableMessaging solo menciona WS-Addressing 2004/08, no restringe la versión de WS-Addressing que se ha de utilizar. A continuación se muestra una lista de restricciones que se aplican a WCF:

- R2101: WS-Addressing 2004/08 y WS-Addressing 1.0 se pueden utilizar con la mensajería de confianza de WS.

- R2102: se debe utilizar una única versión de WS-Addressing a lo largo de una secuencia de WS-ReliableMessaging determinada o un par de secuencias inversas correlacionadas mediante el mecanismo `Offer`.

### <a name="composition-with-soap"></a>Composición con SOAP

WCF admite el uso de SOAP 1,1 y SOAP 1,2 con mensajería de confianza de WS.

### <a name="composition-with-ws-security-and-ws-secureconversation"></a>Composición con WS-Security y WS-SecureConversation

WCF proporciona protección para las secuencias de WS-ReliableMessaging mediante el transporte seguro (HTTPS), la composición con WS-Security y la composición con WS-Secure Conversation. Los protocolos WS-ReliableMessaging 1.1, WS-Security 1.1 y WS-Secure Conversation 1.3 deberían utilizarse conjuntamente. A continuación se muestra una lista de restricciones que se aplican a WCF:

- R2301: para proteger la integridad de una secuencia de WS-ReliableMessaging además de la integridad y confidencialidad de los mensajes individuales, WCF requiere que se utilice WS-Secure Conversation.

- R2302:una sesión de WS-Secure Conversation se debe establecer antes de establecer las secuencias de WS-ReliableMessaging.

- R2303: si la duración de la secuencia de WS-ReliableMessaging supera la duración de la sesión de WS-Secure Conversation, el `SecurityContextToken` establecido mediante WS-Secure Conversation se debe renovar utilizando el enlace de renovación de WS-Secure Conversation correspondiente.

- B2304:La secuencia de WS-ReliableMessaging o un par de secuencias inversas correlacionadas siempre se enlazan a una única sesión de WS-SecureConversation.

- R2305: cuando se crea con WS-Secure Conversation, el respondedor de WCF requiere que el mensaje de `CreateSequence` contenga el elemento `wsse:SecurityTokenReference` y el encabezado `wsrm:UsesSequenceSTR`.

 Ejemplo de encabezado `UsesSequenceSTR`.

```xml
<wsrm:UsesSequenceSTR></wsrm:UsesSequenceSTR>
```

### <a name="composition-with-ssltls-sessions"></a>Composición con sesiones de SSL/TLS

WCF no admite la composición con sesiones SSL/TLS:

- B2401: WCF no genera el encabezado `wsrm:UsesSequenceSSL`.

- R2402: un iniciador de mensajería de confianza no debe enviar un mensaje de `CreateSequence` con un encabezado de `wsrm:UsesSequenceSSL` a un respondedor de WCF.

### <a name="composition-with-ws-policy"></a>Composición con WS-Policy

WCF admite dos versiones de WS-Policy: WS-Policy 1,2 y WS-Policy 1,5.

## <a name="ws-reliablemessaging-ws-policy-assertion"></a>Aserción de WS-Policy de WS-ReliableMessaging

WCF usa la aserción WS-Policy de WS-ReliableMessaging `wsrm:RMAssertion` para describir las capacidades de los extremos. A continuación se muestra una lista de restricciones que se aplican a WCF:

- B3001: WCF asocia `wsrmn:RMAssertion` aserción de WS-Policy a elementos de `wsdl:binding`. WCF admite datos adjuntos para `wsdl:binding` y `wsdl:port` elementos.

- B3002: WCF nunca genera la etiqueta `wsp:Optional`.

- B3003: al obtener acceso a la aserción `wsrmp:RMAssertion` WS-Policy, WCF omite la etiqueta de `wsp:Optional` y trata la Directiva de WS-RM como obligatoria.

- R3004: dado que WCF no se compone con sesiones SSL/TLS, WCF no acepta la Directiva que especifica `wsrmp:SequenceTransportSecurity`.

- B3005: WCF siempre genera el elemento `wsrmp:DeliveryAssurance`.

- B3006: WCF siempre especifica la garantía de entrega `wsrmp:ExactlyOnce`.

- B3007: WCF genera y Lee las siguientes propiedades de la aserción de WS-ReliableMessaging y proporciona control sobre ellas en el`ReliableSessionBindingElement`WCF:

  - `netrmp:InactivityTimeout`

  - `netrmp:AcknowledgementInterval`

  Ejemplo de `RMAssertion`.

  ```xml
  <wsrmp:RMAssertion>
    <wsp:Policy>
      <wsrmp:SequenceSTR/>
      <wsrmp:DeliveryAssurance>
        <wsp:Policy>
          <wsrmp:ExactlyOnce/>
          <wsrmp:InOrder/>
        </wsp:Policy>
      </wsrmp:DeliveryAssurance>
    </wsp:Policy>
    <netrmp:InactivityTimeout Milliseconds="600000"/>
    <netrmp:AcknowledgementInterval Milliseconds="200"/>
  </wsrmp:RMAssertion>
  ```

## <a name="flow-control-ws-reliablemessaging-extension"></a>Extensión de WS-ReliableMessaging de control de flujo

WCF usa la extensibilidad de WS-ReliableMessaging para proporcionar un control adicional más estrecho opcional sobre el flujo de mensajes de secuencia.

El control de flujo se habilita estableciendo la propiedad <xref:System.ServiceModel.Channels.ReliableSessionBindingElement.FlowControlEnabled?displayProperty=nameWithType> en `true`. A continuación se muestra una lista de restricciones que se aplican a WCF:

- B4001: cuando está habilitado el control de flujo de la mensajería de confianza, WCF genera un `netrm:BufferRemaining` elemento en la extensibilidad de elementos del encabezado de `SequenceAcknowledgement`, tal y como se muestra en el ejemplo siguiente.

  ```xml
  <wsrm:SequenceAcknowledgement>
    <wsrm:Identifier>urn:uuid:656652b8-9af2-4e94-9d07-2dc21c05ed27</wsrm:Identifier>
    <wsrm:AcknowledgementRange Upper="1" Lower="1"/>
    <netrm:BufferRemaining>8</netrm:BufferRemaining>
  </wsrm:SequenceAcknowledgement>
  ```

- B4002: incluso cuando está habilitado el control de flujo de mensajería de confianza, WCF no requiere un elemento de `netrm:BufferRemaining` en el encabezado de `SequenceAcknowledgement`.

- B4003: el destino de la mensajería confiable de WCF usa `netrm:BufferRemaining` para indicar cuántos mensajes nuevos puede almacenar en búfer.

- B4004: cuando está habilitado el control de flujo de mensajería de confianza, el origen de mensajería confiable de WCF usa el valor de `netrm:BufferRemaining` para limitar la transmisión de mensajes.

- B4005: WCF genera `netrm:BufferRemaining` valores enteros entre 0 y 4096, ambos incluidos, y Lee valores enteros entre 0 y el valor de `maxInclusive` de `xs:int`214748364, ambos incluidos.

## <a name="message-exchange-patterns"></a>Modelos de intercambio de mensajes

En esta sección se describe el comportamiento de WCF cuando se utiliza WS-ReliableMessaging para diferentes patrones de intercambio de mensajes. Para cada patrón de intercambio de mensajes, se consideran los dos escenarios de implementación siguientes:

- Iniciador no direccionable: el iniciador está tras un firewall; el respondedor solo puede entregar mensajes al iniciador mediante respuestas HTTP.

- Iniciador direccionable: el iniciador y el respondedor pueden enviar solicitudes HTTP; en otras palabras, se pueden establecer dos conexiones HTTP inversas.

### <a name="one-way-non-addressable-initiator"></a>Iniciador unidireccional no direccionable

#### <a name="binding"></a>Enlace

WCF proporciona un patrón de intercambio de mensajes unidireccional utilizando una secuencia sobre un canal HTTP. WCF usa solicitudes HTTP para transmitir todos los mensajes desde el iniciador al respondedor y respuestas HTTP para transmitir todos los mensajes del respondedor al iniciador.

#### <a name="createsequence-exchange"></a>Intercambio de CreateSequence

El iniciador de WCF transmite un mensaje `CreateSequence` sin ningún elemento `Offer` en una solicitud HTTP y espera el mensaje `CreateSequenceResponse` en la respuesta HTTP. El respondedor de WCF crea una secuencia y transmite el mensaje de `CreateSequenceResponse` sin ningún elemento `Accept` en la respuesta HTTP.

#### <a name="sequenceacknowledgement"></a>SequenceAcknowledgement

El iniciador de WCF procesa las confirmaciones en la respuesta de todos los mensajes excepto los mensajes de error y mensajes de `CreateSequence`. El respondedor de WCF siempre transmite una confirmación independiente en la respuesta HTTP a todos los mensajes de secuencia y `AckRequested`.

#### <a name="closesequence-exchange"></a>Intercambio de CloseSequence

El iniciador de WCF transmite un mensaje de `CloseSequence` en una solicitud HTTP y espera el mensaje de `CreateSequenceResponse` en la respuesta HTTP. El respondedor de WCF transmite el mensaje de `CloseSequenceResponse` en la respuesta HTTP.

#### <a name="terminatesequence-exchange"></a>Intercambio de TerminateSequence

El iniciador de WCF transmite un mensaje de `TerminateSequence` en una solicitud HTTP y espera el mensaje de `TerminateSequenceResponse` en la respuesta HTTP. El respondedor de WCF transmite el mensaje de `TerminateSequenceResponse` en la respuesta HTTP.

### <a name="one-way-addressable-initiator"></a>Iniciador unidireccional y direccionable

#### <a name="binding"></a>Enlace

WCF proporciona un patrón de intercambio de mensajes unidireccional utilizando una secuencia sobre un canal HTTP entrante y otro saliente. WCF usa las solicitudes HTTP para transmitir todos los mensajes. Todas las respuestas HTTP tienen un cuerpo vacío y código de estado HTTP 202.

#### <a name="createsequence-exchange"></a>Intercambio de CreateSequence

El iniciador de WCF transmite un mensaje `CreateSequence` sin ningún elemento `Offer` en una solicitud HTTP. El respondedor de WCF crea una secuencia y transmite el mensaje de `CreateSequenceResponse` sin ningún elemento `Accept` en una solicitud HTTP.

### <a name="duplex-addressable-initiator"></a>Iniciador dúplex y direccionable

#### <a name="binding"></a>Enlace

WCF proporciona un patrón de intercambio de mensajes bidireccional totalmente asincrónico que usa dos secuencias sobre un canal HTTP entrante y otro saliente. Este patrón de intercambio de mensajes se puede mezclar con el patrón de intercambio de mensajes del iniciador de `Request/Reply`, `Addressable` de una manera limitada. WCF usa solicitudes HTTP para transmitir todos los mensajes. Todas las respuestas HTTP tienen un cuerpo vacío y código de estado HTTP 202.

#### <a name="createsequence-exchange"></a>Intercambio de CreateSequence

El iniciador de WCF transmite un mensaje `CreateSequence` con un elemento `Offer` en una solicitud HTTP. El respondedor de WCF garantiza que el `CreateSequence` tenga un elemento `Offer` y, a continuación, crea una secuencia y transmite el mensaje `CreateSequenceResponse` con un elemento `Accept`.

#### <a name="sequence-lifetime"></a>Duración de la secuencia

WCF trata las dos secuencias como una sesión totalmente dúplex.

Tras generar un error que genera un error en una secuencia, WCF espera que el punto de conexión remoto genere un error en ambas secuencias. Al leer un error que genera un error en una secuencia, WCF genera un error en ambas secuencias.

WCF puede cerrar su secuencia de salida y continuar procesando los mensajes en la secuencia de entrada. Por el contrario, WCF puede procesar el cierre de la secuencia de entrada y continuar enviando mensajes en su secuencia de salida.

### <a name="request-reply-and-one-way-non-addressable-initiator"></a>Iniciador de solicitud-respuesta unidireccional y no direccionable

#### <a name="binding"></a>Enlace

WCF proporciona un modelo de intercambio de mensajes de solicitud-respuesta unidireccional que usa dos secuencias sobre un canal HTTP. WCF usa solicitudes HTTP para transmitir todos los mensajes desde el iniciador al respondedor y respuestas HTTP para transmitir todos los mensajes del respondedor al iniciador.

#### <a name="createsequence-exchange"></a>Intercambio de CreateSequence

El iniciador de WCF transmite un mensaje `CreateSequence` con un elemento `Offer` en una solicitud HTTP y espera el mensaje `CreateSequenceResponse` en la respuesta HTTP. El respondedor de WCF crea una secuencia y transmite el mensaje de `CreateSequenceResponse` con un elemento `Accept` en la respuesta HTTP.

#### <a name="one-way-message"></a>Mensaje unidireccional

Para completar correctamente un intercambio de mensajes unidireccional, el iniciador de WCF transmite un mensaje de secuencia de solicitud en la solicitud HTTP y recibe un mensaje de `SequenceAcknowledgement` independiente en la respuesta HTTP. La `SequenceAcknowledgement` debe confirmar el mensaje transmitido.

El respondedor de WCF puede responder a la solicitud con una confirmación, un error o una respuesta con un cuerpo vacío y un código de Estado HTTP 202.

#### <a name="two-way-messages"></a>Mensajes bidireccionales

Para completar correctamente un protocolo de intercambio de mensajes bidireccional, el iniciador de WCF transmite un mensaje de secuencia de solicitud en la solicitud HTTP y recibe un mensaje de secuencia de respuesta en la respuesta HTTP. La respuesta debe llevar una `SequenceAcknowledgement` que confirme que se ha transmitido el mensaje de secuencia de solicitud.

El respondedor de WCF puede responder a la solicitud con una respuesta de la aplicación, un error o una respuesta con un cuerpo vacío y un código de Estado HTTP 202.

Debido a la presencia de mensajes unidireccionales y al control de tiempo de las respuestas de la aplicación, el número de secuencia del mensaje de secuencia de solicitud y el número de secuencia del mensaje de respuesta no tienen ninguna correlación.

#### <a name="retrying-replies"></a>Reintento de respuestas

WCF se basa en la correlación de solicitud-respuesta HTTP para la correlación del Protocolo de intercambio de mensajes bidireccional. Debido a esto, el iniciador de WCF no deja de reintentar un mensaje de secuencia de solicitud cuando se confirma el mensaje de la secuencia de solicitud, sino cuando la respuesta HTTP lleva una `SequenceAcknowledgement`, una respuesta de la aplicación o un error. El respondedor de WCF reintenta las respuestas en la respuesta HTTP de la solicitud a la que se correlaciona la respuesta.

#### <a name="closesequence-exchange"></a>Intercambio de CloseSequence

Después de recibir todos los mensajes de secuencia de respuesta y las confirmaciones de todos los mensajes de secuencia de solicitud unidireccional, el iniciador de WCF transmite un mensaje de `CloseSequence` para la secuencia de solicitud en una solicitud HTTP y espera el `CloseSequenceResponse` en la respuesta HTTP.

Al cerrar implícitamente la secuencia de la solicitud, se cierra la secuencia de la respuesta. Esto significa que el iniciador de WCF incluye el `SequenceAcknowledgement` final de la secuencia de respuesta en el mensaje de `CloseSequence` y la secuencia de respuesta no tiene un `CloseSequence` Exchange.

El respondedor de WCF garantiza que todas las respuestas se reconocen y transmite el mensaje de `CloseSequenceResponse` en la respuesta HTTP.

#### <a name="terminatesequence-exchange"></a>Intercambio de TerminateSequence

Después de recibir el mensaje de `CloseSequenceResponse`, el iniciador de WCF transmite un mensaje de `TerminateSequence` para la secuencia de solicitud en una solicitud HTTP y espera la `TerminateSequenceResponse` en la respuesta HTTP.

Al igual que en el intercambio de `CloseSequence`, al finalizar la secuencia de solicitud, se finaliza la secuencia de respuesta. Esto significa que el iniciador de WCF incluye el `SequenceAcknowledgement` final de la secuencia de respuesta en el mensaje de `TerminateSequence` y la secuencia de respuesta no tiene un `TerminateSequence` Exchange.

El respondedor de WCF transmite el mensaje de `TerminateSequenceResponse` en la respuesta HTTP.

### <a name="requestreply-addressable-initiator"></a>Iniciador direccionable de solicitud-respuesta

#### <a name="binding"></a>Enlace

WCF proporciona un patrón de intercambio de mensajes de solicitud-respuesta con dos secuencias sobre un canal HTTP entrante y uno saliente. Este patrón de intercambio de mensajes se puede mezclar con el patrón de intercambio de mensajes del iniciador de `Duplex, Addressable` de una manera limitada. WCF usa las solicitudes HTTP para transmitir todos los mensajes. Todas las respuestas HTTP tienen un cuerpo vacío y código de estado HTTP 202.

#### <a name="createsequence-exchange"></a>Intercambio de CreateSequence

El iniciador de WCF transmite un mensaje `CreateSequence` con un elemento `Offer` en una solicitud HTTP. El respondedor de WCF garantiza que el `CreateSequence` tiene un elemento `Offer`, a continuación, crea una secuencia y transmite el mensaje `CreateSequenceResponse` con un elemento `Accept`.

#### <a name="requestreply-correlation"></a>Correlación entre solicitud y respuesta

Lo siguiente se aplica a todas las solicitudes y respuestas correlacionadas:

- WCF garantiza que todos los mensajes de solicitud de aplicación llevan una referencia de extremo `ReplyTo` y un `MessageId`.

- WCF aplica la referencia de punto de conexión local como `ReplyTo`de cada mensaje de solicitud de aplicación. La referencia del punto de conexión local es la `CreateSequence` del mensaje `ReplyTo` para el iniciador y la `CreateSequence` del mensaje `To` para el respondedor.

- WCF garantiza que los mensajes de solicitud entrantes llevan un `MessageId` y un `ReplyTo`.

- WCF garantiza que el URI de la referencia de extremo de `ReplyTo` de todos los mensajes de solicitud de aplicación coincide con la referencia de punto de conexión local tal como se definió anteriormente.

- WCF garantiza que todas las respuestas llevan los encabezados `RelatesTo` y `To` correctos después de las reglas de correlación de solicitud/respuesta de `wsa`.
