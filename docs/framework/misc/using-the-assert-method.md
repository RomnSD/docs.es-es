---
title: Utilizar el método Assert
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- granting permissions, overriding security checks
- code access security, overriding security checks
- overriding security checks
- security [.NET Framework], overriding security checks
- security [.NET Framework], assertions
- asserted permissions
- Assert method
- caller security checks
- permissions [.NET Framework], overriding security checks
- permissions [.NET Framework], assertions
ms.assetid: 1e40f4d3-fb7d-4f19-b334-b6076d469ea9
ms.openlocfilehash: 92e49af78d42f360d5798a72d4e7b981295947e9
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2020
ms.locfileid: "79181106"
---
# <a name="using-the-assert-method"></a>Utilizar el método Assert
[!INCLUDE[net_security_note](../../../includes/net-security-note-md.md)]  
  
 <xref:System.Security.CodeAccessPermission.Assert%2A> es un método al que se puede llamar en las clases de permiso de acceso a código y en la clase <xref:System.Security.PermissionSet>. Puede usar **Assert** para habilitar el código (y los llamadores de nivel inferior) para realizar acciones que el código tiene permiso para hacer, pero es posible que sus llamadores no tengan permiso para hacerlo. Una aserción o validación de seguridad cambia el proceso normal que realiza el runtime durante una comprobación de seguridad. Al validar un permiso, se indica al sistema de seguridad que no compruebe los llamadores de su código para el permiso validado.  
  
> [!CAUTION]
> Use las aserciones con cuidado, ya que crean vulnerabilidades de seguridad y minan el mecanismo de runtime para imponer las restricciones de seguridad.  
  
 Las aserciones son útiles en situaciones en las que una biblioteca llama a código no administrado o realiza una llamada que requiere un permiso que no está claramente relacionado con el uso previsto de la biblioteca. Por ejemplo, todo el código administrado que llama a código no administrado debe tener **SecurityPermission** con la marca **UnmanagedCode** especificada. No concederá este permiso de forma predeterminada al código que no se origine en el equipo local, como es el caso del código que se descarga desde la intranet local. Por lo tanto, para que el código descargado de la intranet local pueda llamar a una biblioteca que usa código no administrado, la biblioteca debe validar el permiso. Además, algunas bibliotecas podrían realizar llamadas invisibles para los llamadores y que requieren permisos especiales.  
  
 También puede utilizar aserciones en situaciones en las que el código accede a un recurso de tal forma que queda totalmente oculto para los llamadores. Por ejemplo, suponga que su biblioteca adquiere información de una base de datos, pero al hacerlo también lee información del registro del equipo. Dado que los desarrolladores que usan la biblioteca no tienen acceso al origen, no tienen forma de saber que su código requiere **RegistryPermission** para usar el código. En este caso, si decide que no es razonable o necesario exigir que los llamadores del código tengan permiso para acceder al registro, puede validar el permiso de lectura del registro. En esta situación, es adecuado que la biblioteca afirme el permiso para que los llamadores sin **RegistryPermission** puedan usar la biblioteca.  
  
 La aserción afecta al recorrido de pila solo si el permiso validado y un permiso exigido por un llamador indirecto son del mismo tipo y si el permiso exigido es un subconjunto del permiso validado. Por ejemplo, si afirma **FileIOPermission** para leer todos los archivos de la unidad C y se realiza una demanda de nivel inferior para **FileIOPermission** para leer archivos en C:-Temp, la aserción podría afectar al recorrido de la pila; sin embargo, si la demanda era que **FileIOPermission** escribiera en la unidad C, la aserción no tendría ningún efecto.  
  
 Para realizar aserciones, el código debe disponer del permiso que usted está validando y el <xref:System.Security.Permissions.SecurityPermission> que representa el derecho a realizar aserciones. Aunque podría validar un permiso que no se haya concedido al código, la aserción no tendría sentido, ya que la comprobación de seguridad no se podría realizar correctamente antes de que la aserción pudiese lograr que se procesase debidamente.  
  
 En la siguiente ilustración se muestra lo que sucede cuando se utiliza **Assert**. Supongamos que son ciertas las siguientes declaraciones sobre los ensamblados A, B, C, E y F y los permisos P1 y P1A:  
  
- P1A representa el derecho a leer archivos .txt de la unidad C.  
  
- P1 representa el derecho a leer todos los archivos de la unidad C.  
  
- P1A y P1 son ambos tipos **FileIOPermission** y P1A es un subconjunto de P1.  
  
- Se ha concedido el permiso P1A a los ensamblados E y F.  
  
- Se ha concedido el permiso P1 al ensamblado C.  
  
- No se ha concedido ni el permiso P1 ni el P1A a los ensamblados A y B.  
  
- El método A está incluido en el ensamblado A, el método B incluido en el ensamblado B, y así sucesivamente.  
  
 ![Diagrama que muestra los ensamblados del método Assert.](./media/using-the-assert-method/assert-method-assemblies.gif)
  
 En este escenario, el método A llama a B, B llama a C, C llama a E, y E llama a F. El método C afirma el permiso para leer los archivos en la unidad C (permiso P1), y el método E exige permiso para leer los archivos .txt en la unidad C (permiso P1A). Cuando se encuentra la demanda en F en tiempo de ejecución, se realiza un recorrido de pila para comprobar los permisos de todos los llamadores de F, a partir de E. E se le ha concedido el permiso P1A, por lo que el recorrido de pila procede a examinar los permisos de C, donde se detecta la aserción de C. Dado que el permiso solicitado (P1A) es un subconjunto del permiso validado (P1), el recorrido de pila se detiene y se supera automáticamente la comprobación de seguridad. No importa que los ensamblados A y B no dispongan del permiso P1A. Al validar el permiso P1, el método C garantiza que sus llamadores puedan acceder al recurso protegido por P1, aunque los llamadores no tengan permiso para acceder a ese recurso.  
  
 Si diseña una biblioteca de clases y una clase obtiene acceso a un recurso protegido, debería, en la mayoría de los casos, realizar una petición de seguridad que requiera que los llamadores de la clase dispongan del permiso adecuado. Si la clase realiza una operación para la que sabe que la mayoría de sus llamadores no tendrán permiso, y si está dispuesto a asumir la responsabilidad de permitir que estos llamadores llamen al código, puede afirmar el permiso llamando al método **Assert** en un objeto de permiso que representa la operación que está realizando el código. El uso de **Assert** de esta manera permite a los llamadores que normalmente no podrían hacerlo llamar al código. Por tanto, si valida un permiso, debe asegurarse de realizar previamente las comprobaciones de seguridad adecuadas para impedir que el componente se utilice incorrectamente.  
  
 Por ejemplo, suponga que su clase de biblioteca de mucha confianza tiene un método que elimina los archivos. Accede al archivo llamando a una función Win32 no administrada. Un llamador invoca el método **Delete** del código, pasando el nombre del archivo que se va a eliminar, C:-Test.txt. Dentro del método **Delete,** <xref:System.Security.Permissions.FileIOPermission> el código crea un objeto que representa el acceso de escritura a C:-Test.txt. (Se requiere acceso de escritura para eliminar un archivo.) A continuación, el código invoca una comprobación de seguridad imperativa llamando al método **Demand** del objeto **FileIOPermission.** Si uno de los llamadores de la pila de llamadas no tiene este permiso, se genera una <xref:System.Security.SecurityException>. Si no se genera ninguna excepción, sabe que todos los llamadores tienen derecho a acceder a C:\Test.txt. Dado que cree que la mayoría de los llamadores no tendrán <xref:System.Security.Permissions.SecurityPermission> permiso para tener acceso al código no administrado, el código crea un objeto que representa el derecho a llamar a código no administrado y llama al método **Assert** del objeto. Por último, llama a la función de Win32 no administrada para eliminar C:\Text.txt y devuelve el control al llamador.  
  
> [!CAUTION]
> Asegúrese de que su código no utilice aserciones en situaciones en las que su código pueda ser utilizado por otro código para acceder a un recurso que está protegido por el permiso que está validando. Por ejemplo, en el código que escribe en un archivo cuyo nombre especifica el autor de la llamada como parámetro, no se afirma el **FileIOPermission** para escribir en archivos porque el código estaría abierto a un uso indebido por un tercero.  
  
 Cuando se usa la sintaxis de seguridad imperativa, llamar al método **Assert** en varios permisos en el mismo método hace que se produzca una excepción de seguridad. En su lugar, debe crear un **objeto PermissionSet,** pasarle los permisos individuales que desea invocar y, a continuación, llamar al método **Assert** en el objeto **PermissionSet.** Puede llamar al método **Assert** más de una vez cuando utilice la sintaxis de seguridad declarativa.  
  
 En el ejemplo siguiente se muestra la sintaxis declarativa para invalidar comprobaciones de seguridad mediante el **Assert** método. Observe que la sintaxis **FileIOPermissionAttribute** toma dos valores: una enumeración <xref:System.Security.Permissions.SecurityAction> y la ubicación del archivo o directorio al que se va a conceder el permiso. La llamada a **Assert** provoca `C:\Log.txt` demandas de acceso para que el acceso se realice correctamente, aunque los llamadores no se comprueban para obtener permiso para tener acceso al archivo.  
  
```vb  
Option Explicit  
Option Strict  
  
Imports System  
Imports System.IO  
Imports System.Security.Permissions  
  
Namespace LogUtil  
   Public Class Log  
      Public Sub New()  
  
      End Sub  
  
     <FileIOPermission(SecurityAction.Assert, All := "C:\Log.txt")> Public Sub
      MakeLog()  
         Dim TextStream As New StreamWriter("C:\Log.txt")  
         TextStream.WriteLine("This  Log was created on {0}", DateTime.Now) '  
         TextStream.Close()  
      End Sub  
   End Class  
End Namespace  
```  
  
```csharp  
namespace LogUtil  
{  
   using System;  
   using System.IO;  
   using System.Security.Permissions;  
  
   public class Log  
   {  
      public Log()  
      {
      }
      [FileIOPermission(SecurityAction.Assert, All = @"C:\Log.txt")]  
      public void MakeLog()  
      {
         StreamWriter TextStream = new StreamWriter(@"C:\Log.txt");  
         TextStream.WriteLine("This  Log was created on {0}", DateTime.Now);  
         TextStream.Close();  
      }  
   }  
}
```  
  
 Los siguientes fragmentos de código muestran sintaxis imperativa para invalidar comprobaciones de seguridad mediante el **método Assert.** En este ejemplo, se declara una instancia del objeto **FileIOPermission.** Su constructor se pasa **FileIOPermissionAccess.AllAccess** para definir el tipo de acceso permitido, seguido de una cadena que describe la ubicación del archivo. Una vez definido el objeto **FileIOPermission,** solo es necesario llamar a su método **Assert** para invalidar la comprobación de seguridad.  
  
```vb  
Option Explicit  
Option Strict  
Imports System  
Imports System.IO  
Imports System.Security.Permissions  
Namespace LogUtil  
   Public Class Log  
      Public Sub New()  
      End Sub 'New  
  
      Public Sub MakeLog()  
         Dim FilePermission As New FileIOPermission(FileIOPermissionAccess.AllAccess, "C:\Log.txt")  
         FilePermission.Assert()  
         Dim TextStream As New StreamWriter("C:\Log.txt")  
         TextStream.WriteLine("This  Log was created on {0}", DateTime.Now)  
         TextStream.Close()  
      End Sub  
   End Class  
End Namespace  
```  
  
```csharp  
namespace LogUtil  
{  
   using System;  
   using System.IO;  
   using System.Security.Permissions;  
  
   public class Log  
   {  
      public Log()  
      {
      }
      public void MakeLog()  
      {  
         FileIOPermission FilePermission = new FileIOPermission(FileIOPermissionAccess.AllAccess,@"C:\Log.txt");
         FilePermission.Assert();  
         StreamWriter TextStream = new StreamWriter(@"C:\Log.txt");  
         TextStream.WriteLine("This  Log was created on {0}", DateTime.Now);  
         TextStream.Close();  
      }  
   }  
}  
```  
  
## <a name="see-also"></a>Consulte también

- <xref:System.Security.PermissionSet>
- <xref:System.Security.Permissions.SecurityPermission>
- <xref:System.Security.Permissions.FileIOPermission>
- <xref:System.Security.Permissions.SecurityAction>
- [Atributos](../../standard/attributes/index.md)
- [Seguridad de acceso al código](code-access-security.md)
