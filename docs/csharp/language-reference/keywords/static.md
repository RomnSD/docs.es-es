---
title: 'Modificador static: Referencia de C#'
ms.date: 01/22/2020
f1_keywords:
- static
- static_CSharpKeyword
helpviewer_keywords:
- static keyword [C#]
ms.assetid: 5509e215-2183-4da3-bab4-6b7e607a4fdf
ms.openlocfilehash: e7671e9db488a7b50f4ed736864d6fa8d95eef1a
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/14/2020
ms.locfileid: "76744664"
---
# <a name="static-c-reference"></a>static (Referencia de C#)

Use el modificador `static` para declarar un miembro estático, que pertenece al propio tipo en lugar de a un objeto específico. El modificador `static` se puede usar para declarar clases `static`. En las clases, las interfaces y las estructuras, puede agregar el modificador `static` a los campos, los métodos, las propiedades, los operadores, los eventos y los constructores. El modificador `static` no se puede usar con indizadores ni finalizadores. Para obtener más información, consulte [Clases estáticas y sus miembros](../../programming-guide/classes-and-structs/static-classes-and-static-class-members.md).

## <a name="example"></a>Ejemplo

La siguiente clase se declara como `static` y contiene solo métodos `static`:

[!code-csharp[csrefKeywordsModifiers#18](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsModifiers/CS/csrefKeywordsModifiers.cs#18)]

Una declaración de constantes o tipos es implícitamente un miembro `static`. No se puede hacer referencia a un miembro `static` mediante una instancia, sino que se hace a través del nombre de tipo. Por ejemplo, considere la siguiente clase:

[!code-csharp[csrefKeywordsModifiers#19](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsModifiers/CS/csrefKeywordsModifiers.cs#19)]

Para hacer referencia al miembro `static` `x`, use el nombre completo, `MyBaseC.MyStruct.x`, a menos que el miembro sea accesible desde el mismo ámbito:

```csharp
Console.WriteLine(MyBaseC.MyStruct.x);
```

Mientras que una instancia de una clase contiene una copia independiente de todos los campos de instancia de la clase, solo hay una copia de cada campo `static`.

No se puede usar [`this`](this.md) para hacer referencia a métodos `static` o descriptores de acceso de propiedades.

Si la palabra clave `static` se aplica a una clase, todos los miembros de esta deben ser `static`.

Las clases, las interfaces y las clases `static` pueden tener constructores `static`. Se llama a un constructor `static` en algún momento entre el inicio del programa y la creación de una instancia de la clase.

> [!NOTE]
> La palabra clave `static` tiene usos más limitados que en C++. Para ver una comparación con la palabra clave de C++, vea [Clases de almacenamiento (C++)](/cpp/cpp/storage-classes-cpp#static).

Para mostrar miembros `static`, es recomendable una clase que represente al empleado de una empresa. Supongamos que la clase contiene un método de recuento de empleados y un campo para almacenar el número de empleados. El método y el campo no pertenecen a ninguna instancia de ningún empleado, sino que pertenecen a la clase de empleados en su conjunto. Se deben declarar como miembros `static` de la clase.

## <a name="example"></a>Ejemplo

En este ejemplo se lee el nombre y el identificador de un nuevo empleado, se incrementa en uno el recuento de empleados y se muestra la información del nuevo empleado, así como el nuevo número de empleados. Este programa lee el número actual de empleados desde el teclado.

[!code-csharp[csrefKeywordsModifiers#20](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsModifiers/CS/csrefKeywordsModifiers.cs#20)]  

## <a name="example"></a>Ejemplo

En este ejemplo se muestra que se puede inicializar un campo `static` mediante otro campo `static` que aún no se ha declarado. Los resultados están sin definir hasta que se asigna explícitamente un valor al campo `static`.

[!code-csharp[csrefKeywordsModifiers#21](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsModifiers/CS/csrefKeywordsModifiers.cs#21)]  

## <a name="c-language-specification"></a>especificación del lenguaje C#

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>Vea también

- [Referencia de C#](../index.md)
- [Guía de programación de C#](../../programming-guide/index.md)
- [Palabras clave de C#](index.md)
- [Modificadores](index.md)
- [Clases estáticas y sus miembros](../../programming-guide/classes-and-structs/static-classes-and-static-class-members.md)
