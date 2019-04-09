---
title: 'Operadores aritméticos: referencia de C#'
description: Obtenga información sobre los operadores de C# que realizan operaciones de multiplicación, división, resto, suma y resta con tipos numéricos.
ms.date: 03/27/2019
author: pkulikov
f1_keywords:
- ++_CSharpKeyword
- --_CSharpKeyword
- '*_CSharpKeyword'
- /_CSharpKeyword
- '%_CSharpKeyword'
- +_CSharpKeyword
- -_CSharpKeyword
helpviewer_keywords:
- arithmetic operators [C#]
- increment operator [C#]
- ++ operator [C#]
- decrement operator [C#]
- -- operator [C#]
- multiplication operator [C#]
- '* operator [C#]'
- division operator [C#]
- / operator [C#]
- remainder operator [C#]
- '% operator [C#]'
- addition operator [C#]
- + operator [C#]
- subtraction operator [C#]
- '- operator [C#]'
ms.openlocfilehash: 192acf6fea0c6014aaf092077f8deaa844dfd2ec
ms.sourcegitcommit: d938c39afb9216db377d0f0ecdaa53936a851059
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2019
ms.locfileid: "58633808"
---
# <a name="arithmetic-operators-c-reference"></a><span data-ttu-id="f9b6b-103">Operadores aritméticos (referencia de C#)</span><span class="sxs-lookup"><span data-stu-id="f9b6b-103">Arithmetic operators (C# Reference)</span></span>

<span data-ttu-id="f9b6b-104">Los operadores siguientes realizan operaciones aritméticas con tipos numéricos:</span><span class="sxs-lookup"><span data-stu-id="f9b6b-104">The following operators perform arithmetic operations with numeric types:</span></span>

- <span data-ttu-id="f9b6b-105">Operadores unarios [`++` (incremento)](#increment-operator-), [`--` (decremento)](#decrement-operator---), [`+` (más)](#unary-plus-and-minus-operators) y [`-` (menos)](#unary-plus-and-minus-operators).</span><span class="sxs-lookup"><span data-stu-id="f9b6b-105">Unary [`++` (increment)](#increment-operator-), [`--` (decrement)](#decrement-operator---), [`+` (plus)](#unary-plus-and-minus-operators), and [`-` (minus)](#unary-plus-and-minus-operators) operators.</span></span>
- <span data-ttu-id="f9b6b-106">Operadores binarios [`*` (multiplicación)](#multiplication-operator-), [`/` (división)](#division-operator-), [`%` (resto)](#remainder-operator-), [`+` (suma)](#addition-operator-) y [`-` (resta)](#subtraction-operator--).</span><span class="sxs-lookup"><span data-stu-id="f9b6b-106">Binary [`*` (multiplication)](#multiplication-operator-), [`/` (division)](#division-operator-), [`%` (remainder)](#remainder-operator-), [`+` (addition)](#addition-operator-), and [`-` (subtraction)](#subtraction-operator--) operators.</span></span>

<span data-ttu-id="f9b6b-107">Estos operadores admiten todos los tipos numéricos [integrales](../keywords/integral-types-table.md) y de [punto flotante](../keywords/floating-point-types-table.md).</span><span class="sxs-lookup"><span data-stu-id="f9b6b-107">Those operators support all [integral](../keywords/integral-types-table.md) and [floating-point](../keywords/floating-point-types-table.md) numeric types.</span></span>

## <a name="increment-operator-"></a><span data-ttu-id="f9b6b-108">Operador de incremento ++</span><span class="sxs-lookup"><span data-stu-id="f9b6b-108">Increment operator ++</span></span>

<span data-ttu-id="f9b6b-109">El operador de incremento unario `++` incrementa su operando en 1.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-109">The unary increment operator `++` increments its operand by 1.</span></span> <span data-ttu-id="f9b6b-110">El operando debe ser una variable, un acceso de [propiedad](../../programming-guide/classes-and-structs/properties.md) o un acceso de [indexador](../../../csharp/programming-guide/indexers/index.md).</span><span class="sxs-lookup"><span data-stu-id="f9b6b-110">The operand must be a variable, a [property](../../programming-guide/classes-and-structs/properties.md) access, or an [indexer](../../../csharp/programming-guide/indexers/index.md) access.</span></span>

<span data-ttu-id="f9b6b-111">El operador de incremento se admite en dos formas: el operador de incremento posfijo (`x++`) y el operador de incremento prefijo (`++x`).</span><span class="sxs-lookup"><span data-stu-id="f9b6b-111">The increment operator is supported in two forms: the postfix increment operator, `x++`, and the prefix increment operator, `++x`.</span></span>

### <a name="postfix-increment-operator"></a><span data-ttu-id="f9b6b-112">Operador de incremento de postfijo</span><span class="sxs-lookup"><span data-stu-id="f9b6b-112">Postfix increment operator</span></span>

<span data-ttu-id="f9b6b-113">El resultado de `x++` es el valor de `x` *antes* de la operación, tal y como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="f9b6b-113">The result of `x++` is the value of `x` *before* the operation, as the following example shows:</span></span>

[!code-csharp-interactive[postfix increment](~/samples/snippets/csharp/language-reference/operators/ArithmeticOperators.cs#PostfixIncrement)]

### <a name="prefix-increment-operator"></a><span data-ttu-id="f9b6b-114">Operador de incremento prefijo</span><span class="sxs-lookup"><span data-stu-id="f9b6b-114">Prefix increment operator</span></span>

<span data-ttu-id="f9b6b-115">El resultado de `++x` es el valor de `x` *después* de la operación, tal y como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="f9b6b-115">The result of `++x` is the value of `x` *after* the operation, as the following example shows:</span></span>

[!code-csharp-interactive[prefix increment](~/samples/snippets/csharp/language-reference/operators/ArithmeticOperators.cs#PrefixIncrement)]

## <a name="decrement-operator---"></a><span data-ttu-id="f9b6b-116">Operador de decremento --</span><span class="sxs-lookup"><span data-stu-id="f9b6b-116">Decrement operator --</span></span>

<span data-ttu-id="f9b6b-117">El operador de decremento unario `--` disminuye su operando en 1.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-117">The unary decrement operator `--` decrements its operand by 1.</span></span> <span data-ttu-id="f9b6b-118">El operando debe ser una variable, un acceso de [propiedad](../../programming-guide/classes-and-structs/properties.md) o un acceso de [indexador](../../../csharp/programming-guide/indexers/index.md).</span><span class="sxs-lookup"><span data-stu-id="f9b6b-118">The operand must be a variable, a [property](../../programming-guide/classes-and-structs/properties.md) access, or an [indexer](../../../csharp/programming-guide/indexers/index.md) access.</span></span>

<span data-ttu-id="f9b6b-119">El operador de decremento se admite en dos formas: el operador de decremento posfijo (`x--`) y el operador de decremento prefijo (`--x`).</span><span class="sxs-lookup"><span data-stu-id="f9b6b-119">The decrement operator is supported in two forms: the postfix decrement operator, `x--`, and the prefix decrement operator, `--x`.</span></span>

### <a name="postfix-decrement-operator"></a><span data-ttu-id="f9b6b-120">Operador de decremento de postfijo</span><span class="sxs-lookup"><span data-stu-id="f9b6b-120">Postfix decrement operator</span></span>

<span data-ttu-id="f9b6b-121">El resultado de `x--` es el valor de `x` *antes* de la operación, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="f9b6b-121">The result of `x--` is the value of `x` *before* the operation, as the following example shows:</span></span>

[!code-csharp-interactive[postfix decrement](~/samples/snippets/csharp/language-reference/operators/ArithmeticOperators.cs#PostfixDecrement)]

### <a name="prefix-decrement-operator"></a><span data-ttu-id="f9b6b-122">Operador de decremento de prefijo</span><span class="sxs-lookup"><span data-stu-id="f9b6b-122">Prefix decrement operator</span></span>

<span data-ttu-id="f9b6b-123">El resultado de `--x` es el valor de `x` *después* de la operación, tal y como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="f9b6b-123">The result of `--x` is the value of `x` *after* the operation, as the following example shows:</span></span>

[!code-csharp-interactive[prefix decrement](~/samples/snippets/csharp/language-reference/operators/ArithmeticOperators.cs#PrefixDecrement)]

## <a name="unary-plus-and-minus-operators"></a><span data-ttu-id="f9b6b-124">Operadores unarios más y menos</span><span class="sxs-lookup"><span data-stu-id="f9b6b-124">Unary plus and minus operators</span></span>

<span data-ttu-id="f9b6b-125">El operador `+` unario devuelve el valor de su operando.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-125">The unary `+` operator returns the value of its operand.</span></span> <span data-ttu-id="f9b6b-126">El operador unario `-` calcula la negación numérica del operando.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-126">The unary `-` operator computes the numeric negation of its operand.</span></span>

[!code-csharp-interactive[unary plus and minus](~/samples/snippets/csharp/language-reference/operators/ArithmeticOperators.cs#UnaryPlusAndMinus)]

<span data-ttu-id="f9b6b-127">El operador unario `-` no es compatible con el tipo [ulong](../keywords/ulong.md).</span><span class="sxs-lookup"><span data-stu-id="f9b6b-127">The unary `-` operator doesn't support the [ulong](../keywords/ulong.md) type.</span></span>

## <a name="multiplication-operator-"></a><span data-ttu-id="f9b6b-128">Operador de multiplicación \*</span><span class="sxs-lookup"><span data-stu-id="f9b6b-128">Multiplication operator \*</span></span>

<span data-ttu-id="f9b6b-129">El operador de multiplicación `*` calcula el producto de sus operandos:</span><span class="sxs-lookup"><span data-stu-id="f9b6b-129">The multiplication operator `*` computes the product of its operands:</span></span>

[!code-csharp-interactive[multiplication operator](~/samples/snippets/csharp/language-reference/operators/ArithmeticOperators.cs#Multiplication)]

<span data-ttu-id="f9b6b-130">El operador unario `*` es un [operador de direccionamiento indirecto del puntero](multiplication-operator.md#pointer-indirection-operator).</span><span class="sxs-lookup"><span data-stu-id="f9b6b-130">The unary `*` operator is a [pointer indirection operator](multiplication-operator.md#pointer-indirection-operator).</span></span>

## <a name="division-operator-"></a><span data-ttu-id="f9b6b-131">Operador de división /</span><span class="sxs-lookup"><span data-stu-id="f9b6b-131">Division operator /</span></span>

<span data-ttu-id="f9b6b-132">El operador de división `/` divide su primer operando por su segundo operando.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-132">The division operator `/` divides its first operand by its second operand.</span></span>

### <a name="integer-division"></a><span data-ttu-id="f9b6b-133">División de enteros</span><span class="sxs-lookup"><span data-stu-id="f9b6b-133">Integer division</span></span>

<span data-ttu-id="f9b6b-134">Para los operandos de tipos enteros, el resultado del operador `/` es de un tipo entero y equivale al cociente de los dos operandos redondeados hacia cero:</span><span class="sxs-lookup"><span data-stu-id="f9b6b-134">For the operands of integer types, the result of the `/` operator is of an integer type and equals the quotient of the two operands rounded towards zero:</span></span>

[!code-csharp-interactive[integer division](~/samples/snippets/csharp/language-reference/operators/ArithmeticOperators.cs#IntegerDivision)]

<span data-ttu-id="f9b6b-135">Para obtener el cociente de los dos operandos como número de punto flotante, use el tipo `float`, `double` o `decimal`:</span><span class="sxs-lookup"><span data-stu-id="f9b6b-135">To obtain the quotient of the two operands as a floating-point number, use the `float`, `double`, or `decimal` type:</span></span>

[!code-csharp-interactive[integer as floating-point division](~/samples/snippets/csharp/language-reference/operators/ArithmeticOperators.cs#IntegerAsFloatingPointDivision)]

### <a name="floating-point-division"></a><span data-ttu-id="f9b6b-136">División de punto flotante</span><span class="sxs-lookup"><span data-stu-id="f9b6b-136">Floating-point division</span></span>

<span data-ttu-id="f9b6b-137">Para los tipos `float`, `double` y `decimal`, el resultado del operador `/` es el cociente de los dos operandos:</span><span class="sxs-lookup"><span data-stu-id="f9b6b-137">For the `float`, `double`, and `decimal` types, the result of the `/` operator is the quotient of the two operands:</span></span>

[!code-csharp-interactive[floating-point division](~/samples/snippets/csharp/language-reference/operators/ArithmeticOperators.cs#FloatingPointDivision)]

<span data-ttu-id="f9b6b-138">Si uno de los operandos es `decimal`, otro operando no puede ser `float` ni `double`, ya que ni `float` ni `double` se convierte de forma implícita a `decimal`.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-138">If one of the operands is `decimal`, another operand can be neither `float` nor `double`, because neither `float` nor `double` is implicitly convertible to `decimal`.</span></span> <span data-ttu-id="f9b6b-139">Debe convertir explícitamente el operando `float` o `double` al tipo `decimal`.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-139">You must explicitly convert the `float` or `double` operand to the `decimal` type.</span></span> <span data-ttu-id="f9b6b-140">Para obtener más información sobre las conversiones implícitas entre tipos numéricos, consulte [Tabla de conversiones numéricas implícitas](../keywords/implicit-numeric-conversions-table.md).</span><span class="sxs-lookup"><span data-stu-id="f9b6b-140">For more information about implicit conversions between numeric types, see [Implicit numeric conversions table](../keywords/implicit-numeric-conversions-table.md).</span></span>

## <a name="remainder-operator-"></a><span data-ttu-id="f9b6b-141">Operador de resto %</span><span class="sxs-lookup"><span data-stu-id="f9b6b-141">Remainder operator %</span></span>

<span data-ttu-id="f9b6b-142">El operador de resto `%` calcula el resto después de dividir su primer operando entre el segundo.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-142">The remainder operator `%` computes the remainder after dividing its first operand by its second operand.</span></span>

### <a name="integer-remainder"></a><span data-ttu-id="f9b6b-143">Resto entero</span><span class="sxs-lookup"><span data-stu-id="f9b6b-143">Integer remainder</span></span>
  
<span data-ttu-id="f9b6b-144">En el caso de los operandos de tipos enteros, el resultado de `a % b` es el valor producido por `a - (a / b) * b`.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-144">For the operands of integer types, the result of `a % b` is the value produced by `a - (a / b) * b`.</span></span> <span data-ttu-id="f9b6b-145">El signo de resto distinto de cero es el mismo que el del primer operando, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="f9b6b-145">The sign of the non-zero remainder is the same as that of the first operand, as the following example shows:</span></span>

[!code-csharp-interactive[integer remainder](~/samples/snippets/csharp/language-reference/operators/ArithmeticOperators.cs#IntegerRemainder)]

<span data-ttu-id="f9b6b-146">Use el método <xref:System.Math.DivRem%2A?displayProperty=nameWithType> para calcular los resultados de la división de enteros y del resto.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-146">Use the <xref:System.Math.DivRem%2A?displayProperty=nameWithType> method to compute both integer division and remainder results.</span></span>

### <a name="floating-point-remainder"></a><span data-ttu-id="f9b6b-147">Resto de punto flotante</span><span class="sxs-lookup"><span data-stu-id="f9b6b-147">Floating-point remainder</span></span>

<span data-ttu-id="f9b6b-148">En el caso de los operandos `float` y `double`, el resultado de `x % y` para `x` e `y` finitos es el valor `z`, de modo que</span><span class="sxs-lookup"><span data-stu-id="f9b6b-148">For the `float` and `double` operands, the result of `x % y` for the finite `x` and `y` is the value `z` such that</span></span>

- <span data-ttu-id="f9b6b-149">el signo de `z`, si no es cero, es el mismo que el signo de `x`;</span><span class="sxs-lookup"><span data-stu-id="f9b6b-149">The sign of `z`, if non-zero, is the same as the sign of `x`.</span></span>
- <span data-ttu-id="f9b6b-150">el valor absoluto de `z` es el valor producido por `|x| - n * |y|`, donde `n` es el entero más grande posible que sea menor o igual que `|x| / |y|`, y `|x|` e `|y|` son los valores absolutos de `x` e `y`, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-150">The absolute value of `z` is the value produced by `|x| - n * |y|` where `n` is the largest possible integer that is less than or equal to `|x| / |y|` and `|x|` and `|y|` are the absolute values of `x` and `y`, respectively.</span></span>

> [!NOTE]
> <span data-ttu-id="f9b6b-151">Este método de cálculo del resto es análogo al usado para los operandos enteros, pero difiere de IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-151">This method of computing the remainder is analogous to that used for integer operands, but differs from the IEEE 754.</span></span> <span data-ttu-id="f9b6b-152">Si necesita la operación de resto conforme con IEEE 754, use el método <xref:System.Math.IEEERemainder%2A?displayProperty=nameWithType>.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-152">If you need the remainder operation that complies with the IEEE 754, use the <xref:System.Math.IEEERemainder%2A?displayProperty=nameWithType> method.</span></span>

<span data-ttu-id="f9b6b-153">Para información sobre el comportamiento del operador `%` con operandos no finitos, vea la sección [Operador de resto](~/_csharplang/spec/expressions.md#remainder-operator) de la [Especificación del lenguaje C#](~/_csharplang/spec/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f9b6b-153">For information about the behavior of the `%` operator with non-finite operands, see the [Remainder operator](~/_csharplang/spec/expressions.md#remainder-operator) section of the [C# language specification](~/_csharplang/spec/introduction.md).</span></span>

<span data-ttu-id="f9b6b-154">En el caso de los operandos `decimal`, el operador de resto `%` es equivalente al [operador de resto](<xref:System.Decimal.op_Modulus(System.Decimal,System.Decimal)>) del tipo <xref:System.Decimal?displayProperty=nameWithType>.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-154">For the `decimal` operands, the remainder operator `%` is equivalent to the [remainder operator](<xref:System.Decimal.op_Modulus(System.Decimal,System.Decimal)>) of the <xref:System.Decimal?displayProperty=nameWithType> type.</span></span>

<span data-ttu-id="f9b6b-155">En el ejemplo siguiente se muestra el comportamiento del operador de resto con operandos de punto flotante:</span><span class="sxs-lookup"><span data-stu-id="f9b6b-155">The following example demonstrates the behavior of the remainder operator with floating-point operands:</span></span>

[!code-csharp-interactive[floating-point remainder](~/samples/snippets/csharp/language-reference/operators/ArithmeticOperators.cs#FloatingPointRemainder)]

## <a name="addition-operator-"></a><span data-ttu-id="f9b6b-156">Operador de suma +</span><span class="sxs-lookup"><span data-stu-id="f9b6b-156">Addition operator +</span></span>

<span data-ttu-id="f9b6b-157">El operador de suma `+` calcula la suma de sus operandos:</span><span class="sxs-lookup"><span data-stu-id="f9b6b-157">The addition operator `+` computes the sum of its operands:</span></span>

[!code-csharp-interactive[addition operator](~/samples/snippets/csharp/language-reference/operators/ArithmeticOperators.cs#Addition)]

<span data-ttu-id="f9b6b-158">También puede usar el operador `+` para la concatenación de cadenas y la combinación de delegados.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-158">You also can use the `+` operator for string concatenation and delegate combination.</span></span> <span data-ttu-id="f9b6b-159">Para obtener más información, vea el artículo [Operador `+`](addition-operator.md).</span><span class="sxs-lookup"><span data-stu-id="f9b6b-159">For more information, see the [`+` operator](addition-operator.md) article.</span></span>

## <a name="subtraction-operator--"></a><span data-ttu-id="f9b6b-160">Operador de resta -</span><span class="sxs-lookup"><span data-stu-id="f9b6b-160">Subtraction operator -</span></span>

<span data-ttu-id="f9b6b-161">El operador de resta `-` resta su segundo operando del primero:</span><span class="sxs-lookup"><span data-stu-id="f9b6b-161">The subtraction operator `-` subtracts its second operand from its first operand:</span></span>

[!code-csharp-interactive[subtraction operator](~/samples/snippets/csharp/language-reference/operators/ArithmeticOperators.cs#Subtraction)]

<span data-ttu-id="f9b6b-162">También puede usar el operador `-` para la eliminación de delegados.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-162">You also can use the `-` operator for delegate removal.</span></span> <span data-ttu-id="f9b6b-163">Para obtener más información, vea el artículo [Operador `-`](subtraction-operator.md).</span><span class="sxs-lookup"><span data-stu-id="f9b6b-163">For more information, see the [`-` operator](subtraction-operator.md) article.</span></span>

## <a name="operator-precedence-and-associativity"></a><span data-ttu-id="f9b6b-164">Prioridad y asociatividad de los operadores</span><span class="sxs-lookup"><span data-stu-id="f9b6b-164">Operator precedence and associativity</span></span>

<span data-ttu-id="f9b6b-165">En la lista siguiente se ordenan los operadores aritméticos de la prioridad más alta a la más baja:</span><span class="sxs-lookup"><span data-stu-id="f9b6b-165">The following list orders arithmetic operators starting from the highest precedence to the lowest:</span></span>

- <span data-ttu-id="f9b6b-166">Operadores de incremento `x++` y decremento `x--` posfijos.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-166">Postfix increment `x++` and decrement `x--` operators.</span></span>
- <span data-ttu-id="f9b6b-167">Operadores de incremento `++x` y decremento `--x` prefijos, y operadores unarios `+` y `-`.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-167">Prefix increment `++x` and decrement `--x` and unary `+` and `-` operators.</span></span>
- <span data-ttu-id="f9b6b-168">Operadores de multiplicación `*`, `/` y `%`.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-168">Multiplicative `*`, `/`, and `%` operators.</span></span>
- <span data-ttu-id="f9b6b-169">Operadores de suma adición `+` y `-`.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-169">Additive `+` and `-` operators.</span></span>

<span data-ttu-id="f9b6b-170">Los operadores aritméticos binarios son asociativos a la izquierda.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-170">Binary arithmetic operators are left-associative.</span></span> <span data-ttu-id="f9b6b-171">Es decir, los operadores con el mismo nivel de prioridad se evalúan de izquierda a derecha.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-171">That is, operators with the same precedence level are evaluated from left to right.</span></span>

<span data-ttu-id="f9b6b-172">Use los paréntesis, `()`, para cambiar el orden de evaluación impuesto por la prioridad y la asociatividad de operadores.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-172">Use parentheses, `()`, to change the order of evaluation imposed by operator precedence and associativity.</span></span>

[!code-csharp-interactive[precedence and associativity](~/samples/snippets/csharp/language-reference/operators/ArithmeticOperators.cs#PrecedenceAndAssociativity)]

<span data-ttu-id="f9b6b-173">Para obtener la lista completa de los operadores de C# ordenados por nivel de prioridad, vea [Operadores de C#](index.md).</span><span class="sxs-lookup"><span data-stu-id="f9b6b-173">For the complete list of C# operators ordered by precedence level, see [C# operators](index.md).</span></span>

## <a name="compound-assignment"></a><span data-ttu-id="f9b6b-174">Asignación compuesta</span><span class="sxs-lookup"><span data-stu-id="f9b6b-174">Compound assignment</span></span>

<span data-ttu-id="f9b6b-175">Para un operador binario `op`, una expresión de asignación compuesta con el formato</span><span class="sxs-lookup"><span data-stu-id="f9b6b-175">For a binary operator `op`, a compound assignment expression of the form</span></span>

```csharp
x op= y
```

<span data-ttu-id="f9b6b-176">es equivalente a</span><span class="sxs-lookup"><span data-stu-id="f9b6b-176">is equivalent to</span></span>

```csharp
x = x op y
```

<span data-ttu-id="f9b6b-177">salvo que `x` solo se evalúa una vez.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-177">except that `x` is only evaluated once.</span></span>

<span data-ttu-id="f9b6b-178">En el ejemplo siguiente se muestra el uso de la asignación compuesta con operadores aritméticos:</span><span class="sxs-lookup"><span data-stu-id="f9b6b-178">The following example demonstrates the usage of compound assignment with arithmetic operators:</span></span>

[!code-csharp-interactive[compound assignment](~/samples/snippets/csharp/language-reference/operators/ArithmeticOperators.cs#CompoundAssignment)]

<span data-ttu-id="f9b6b-179">Los operadores `+=` y `-=` también se usan para suscribirse y cancelar la suscripción a [eventos](../keywords/event.md).</span><span class="sxs-lookup"><span data-stu-id="f9b6b-179">You also use the `+=` and `-=` operators to subscribe to and unsubscribe from [events](../keywords/event.md).</span></span> <span data-ttu-id="f9b6b-180">Para obtener más información, vea [Procedimientos para suscribir y cancelar la suscripción a eventos](../../programming-guide/events/how-to-subscribe-to-and-unsubscribe-from-events.md).</span><span class="sxs-lookup"><span data-stu-id="f9b6b-180">For more information, see [How to: subscribe to and unsubscribe from events](../../programming-guide/events/how-to-subscribe-to-and-unsubscribe-from-events.md).</span></span>

## <a name="arithmetic-overflow-and-division-by-zero"></a><span data-ttu-id="f9b6b-181">Desbordamiento aritmético y división por cero</span><span class="sxs-lookup"><span data-stu-id="f9b6b-181">Arithmetic overflow and division by zero</span></span>

<span data-ttu-id="f9b6b-182">Si el resultado de una operación aritmética está fuera del intervalo de valores finitos posibles del tipo numérico implicado, el comportamiento de un operador aritmético depende del tipo de sus operandos.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-182">When the result of an arithmetic operation is outside the range of possible finite values of the involved numeric type, the behavior of an arithmetic operator depends on the type of its operands.</span></span>

### <a name="integer-arithmetic-overflow"></a><span data-ttu-id="f9b6b-183">Desbordamiento aritmético de enteros</span><span class="sxs-lookup"><span data-stu-id="f9b6b-183">Integer arithmetic overflow</span></span>

<span data-ttu-id="f9b6b-184">La división de enteros por cero siempre produce una <xref:System.DivideByZeroException>.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-184">Integer division by zero always throws a <xref:System.DivideByZeroException>.</span></span>

<span data-ttu-id="f9b6b-185">En el caso de desbordamiento aritmético de enteros, el comportamiento resultante lo controla un contexto de comprobación de desbordamiento, que puede ser [comprobado o no comprobado](../keywords/checked-and-unchecked.md):</span><span class="sxs-lookup"><span data-stu-id="f9b6b-185">In case of integer arithmetic overflow, an overflow checking context, which can be [checked or unchecked](../keywords/checked-and-unchecked.md), controls the resulting behavior:</span></span>

- <span data-ttu-id="f9b6b-186">En un contexto comprobado, si el desbordamiento se produce en una expresión constante, se produce un error de compilación.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-186">In a checked context, if overflow happens in a constant expression, a compile-time error occurs.</span></span> <span data-ttu-id="f9b6b-187">De lo contrario, si la operación se realiza en tiempo de ejecución, se inicia una excepción <xref:System.OverflowException>.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-187">Otherwise, when the operation is performed at run time, an <xref:System.OverflowException> is thrown.</span></span>
- <span data-ttu-id="f9b6b-188">En un contexto no comprobado, el resultado se trunca mediante el descarte de los bits de orden superior que no caben en el tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-188">In an unchecked context, the result is truncated by discarding any high-order bits that don't fit in the destination type.</span></span>

<span data-ttu-id="f9b6b-189">Junto con las instrucciones [comprobadas y no comprobadas](../keywords/checked-and-unchecked.md), puede usar los operadores `checked` y `unchecked` para controlar el contexto de comprobación de desbordamiento, en el que se evalúa una expresión:</span><span class="sxs-lookup"><span data-stu-id="f9b6b-189">Along with the [checked and unchecked](../keywords/checked-and-unchecked.md) statements, you can use the `checked` and `unchecked` operators to control the overflow checking context, in which an expression is evaluated:</span></span>

[!code-csharp-interactive[checked and unchecked](~/samples/snippets/csharp/language-reference/operators/ArithmeticOperators.cs#CheckedUnchecked)]

<span data-ttu-id="f9b6b-190">De forma predeterminada, las operaciones aritméticas se producen en un contexto *no comprobado*.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-190">By default, arithmetic operations occur in an *unchecked* context.</span></span>

### <a name="floating-point-arithmetic-overflow"></a><span data-ttu-id="f9b6b-191">Desbordamiento aritmético de punto flotante</span><span class="sxs-lookup"><span data-stu-id="f9b6b-191">Floating-point arithmetic overflow</span></span>

<span data-ttu-id="f9b6b-192">Las operaciones aritméticas con los tipos `float` y `double` nunca inician una excepción.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-192">Arithmetic operations with the `float` and `double` types never throw an exception.</span></span> <span data-ttu-id="f9b6b-193">El resultado de las operaciones aritméticas con esos tipos puede ser uno de varios valores especiales que representan infinito y no un número:</span><span class="sxs-lookup"><span data-stu-id="f9b6b-193">The result of arithmetic operations with those types can be one of special values that represent infinity and not-a-number:</span></span>

[!code-csharp-interactive[double non-finite values](~/samples/snippets/csharp/language-reference/operators/ArithmeticOperators.cs#FloatingPointOverflow)]

<span data-ttu-id="f9b6b-194">Para los operandos del tipo `decimal`, el desbordamiento aritmético siempre inicia una excepción <xref:System.OverflowException> y la división por cero siempre inicia una excepción <xref:System.DivideByZeroException>.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-194">For the operands of the `decimal` type, arithmetic overflow always throws an <xref:System.OverflowException> and division by zero always throws a <xref:System.DivideByZeroException>.</span></span>

## <a name="round-off-errors"></a><span data-ttu-id="f9b6b-195">Errores de redondeo</span><span class="sxs-lookup"><span data-stu-id="f9b6b-195">Round-off errors</span></span>

<span data-ttu-id="f9b6b-196">Debido a las limitaciones generales de la representación de punto flotante de los números reales y la aritmética de punto flotante, en los cálculos con tipos de punto flotante se pueden producir errores de redondeo.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-196">Because of general limitations of the floating-point representation of real numbers and floating-point arithmetic, the round-off errors might occur in calculations with floating-point types.</span></span> <span data-ttu-id="f9b6b-197">Es decir, es posible que el resultado de una expresión difiera del resultado matemático esperado.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-197">That is, the produced result of an expression might differ from the expected mathematical result.</span></span> <span data-ttu-id="f9b6b-198">En el ejemplo siguiente se muestran varios de estos casos:</span><span class="sxs-lookup"><span data-stu-id="f9b6b-198">The following example demonstrates several such cases:</span></span>

[!code-csharp-interactive[round-off errors](~/samples/snippets/csharp/language-reference/operators/ArithmeticOperators.cs#RoundOffErrors)]

<span data-ttu-id="f9b6b-199">Para obtener más información, vea los comentarios en las páginas de referencia de [System.Double](/dotnet/api/system.double#remarks), [System.Single](/dotnet/api/system.single#remarks) o [System.Decimal](/dotnet/api/system.decimal#remarks).</span><span class="sxs-lookup"><span data-stu-id="f9b6b-199">For more information, see remarks at [System.Double](/dotnet/api/system.double#remarks), [System.Single](/dotnet/api/system.single#remarks), or [System.Decimal](/dotnet/api/system.decimal#remarks) reference pages.</span></span>

## <a name="operator-overloadability"></a><span data-ttu-id="f9b6b-200">Posibilidad de sobrecarga del operador</span><span class="sxs-lookup"><span data-stu-id="f9b6b-200">Operator overloadability</span></span>

<span data-ttu-id="f9b6b-201">Los tipos definidos por el usuario pueden [sobrecargar](../keywords/operator.md) los operadores unarios (`++`, `--`, `+` y `-`), los operadores binarios (`*`, `/`, `%`, `+` y `-`) y los operadores aritméticos.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-201">User-defined types can [overload](../keywords/operator.md) the unary (`++`, `--`, `+`, and `-`) and binary (`*`, `/`, `%`, `+`, and `-`) arithmetic operators.</span></span> <span data-ttu-id="f9b6b-202">Cuando se sobrecarga un operador binario, también se sobrecarga de forma implícita el operador de asignación compuesta correspondiente.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-202">When a binary operator is overloaded, the corresponding compound assignment operator is also implicitly overloaded.</span></span> <span data-ttu-id="f9b6b-203">Un tipo definido por el usuario no puede sobrecargar de forma explícita un operador de asignación compuesta.</span><span class="sxs-lookup"><span data-stu-id="f9b6b-203">A user-defined type cannot explicitly overload a compound assignment operator.</span></span>

## <a name="c-language-specification"></a><span data-ttu-id="f9b6b-204">Especificación del lenguaje C#</span><span class="sxs-lookup"><span data-stu-id="f9b6b-204">C# language specification</span></span>

<span data-ttu-id="f9b6b-205">Para más información, vea las secciones siguientes de la [Especificación del lenguaje C#](~/_csharplang/spec/introduction.md):</span><span class="sxs-lookup"><span data-stu-id="f9b6b-205">For more information, see the following sections of the [C# language specification](~/_csharplang/spec/introduction.md):</span></span>

- [<span data-ttu-id="f9b6b-206">Operadores de incremento y decremento posfijos</span><span class="sxs-lookup"><span data-stu-id="f9b6b-206">Postfix increment and decrement operators</span></span>](~/_csharplang/spec/expressions.md#postfix-increment-and-decrement-operators)
- [<span data-ttu-id="f9b6b-207">Operadores de incremento y decremento prefijos</span><span class="sxs-lookup"><span data-stu-id="f9b6b-207">Prefix increment and decrement operators</span></span>](~/_csharplang/spec/expressions.md#prefix-increment-and-decrement-operators)
- [<span data-ttu-id="f9b6b-208">Operador unario más</span><span class="sxs-lookup"><span data-stu-id="f9b6b-208">Unary plus operator</span></span>](~/_csharplang/spec/expressions.md#unary-plus-operator)
- [<span data-ttu-id="f9b6b-209">Operador unario menos</span><span class="sxs-lookup"><span data-stu-id="f9b6b-209">Unary minus operator</span></span>](~/_csharplang/spec/expressions.md#unary-minus-operator)
- [<span data-ttu-id="f9b6b-210">Operador de multiplicación</span><span class="sxs-lookup"><span data-stu-id="f9b6b-210">Multiplication operator</span></span>](~/_csharplang/spec/expressions.md#multiplication-operator)
- [<span data-ttu-id="f9b6b-211">Operador de división</span><span class="sxs-lookup"><span data-stu-id="f9b6b-211">Division operator</span></span>](~/_csharplang/spec/expressions.md#division-operator)
- [<span data-ttu-id="f9b6b-212">Operador de resto</span><span class="sxs-lookup"><span data-stu-id="f9b6b-212">Remainder operator</span></span>](~/_csharplang/spec/expressions.md#remainder-operator)
- [<span data-ttu-id="f9b6b-213">Operador de suma</span><span class="sxs-lookup"><span data-stu-id="f9b6b-213">Addition operator</span></span>](~/_csharplang/spec/expressions.md#addition-operator)
- [<span data-ttu-id="f9b6b-214">Operador de resta</span><span class="sxs-lookup"><span data-stu-id="f9b6b-214">Subtraction operator</span></span>](~/_csharplang/spec/expressions.md#subtraction-operator)
- [<span data-ttu-id="f9b6b-215">Asignación compuesta</span><span class="sxs-lookup"><span data-stu-id="f9b6b-215">Compound assignment</span></span>](~/_csharplang/spec/expressions.md#compound-assignment)
- [<span data-ttu-id="f9b6b-216">Operadores checked y unchecked</span><span class="sxs-lookup"><span data-stu-id="f9b6b-216">The checked and unchecked operators</span></span>](~/_csharplang/spec/expressions.md#the-checked-and-unchecked-operators)

## <a name="see-also"></a><span data-ttu-id="f9b6b-217">Vea también</span><span class="sxs-lookup"><span data-stu-id="f9b6b-217">See also</span></span>

- [<span data-ttu-id="f9b6b-218">Referencia de C#</span><span class="sxs-lookup"><span data-stu-id="f9b6b-218">C# Reference</span></span>](../index.md)
- [<span data-ttu-id="f9b6b-219">Guía de programación de C#</span><span class="sxs-lookup"><span data-stu-id="f9b6b-219">C# Programming Guide</span></span>](../../programming-guide/index.md)
- [<span data-ttu-id="f9b6b-220">Operadores de C#</span><span class="sxs-lookup"><span data-stu-id="f9b6b-220">C# Operators</span></span>](index.md)
- <xref:System.Math?displayProperty=nameWithType>
- <xref:System.MathF?displayProperty=nameWithType>
- [<span data-ttu-id="f9b6b-221">Valores numéricos en .NET</span><span class="sxs-lookup"><span data-stu-id="f9b6b-221">Numerics in .NET</span></span>](../../../standard/numerics.md)