---
title: Publicación de un modelo de Machine Learning en ASP.NET Core Web API
description: Publicación del modelo de Machine Learning de Análisis de sentimiento de ML.NET en Internet mediante ASP.NET Core Web API
ms.date: 03/05/2019
ms.custom: mvc,how-to
ms.openlocfilehash: 07b751caff8ef0ca9a23bed68ddf88feb7b5ae4f
ms.sourcegitcommit: 69bf8b719d4c289eec7b45336d0b933dd7927841
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/14/2019
ms.locfileid: "57856711"
---
# <a name="how-to-serve-machine-learning-model-through-aspnet-core-web-api"></a><span data-ttu-id="09edc-103">Publicación de un modelo de Machine Learning a través de ASP.NET Core Web API</span><span class="sxs-lookup"><span data-stu-id="09edc-103">How-To: Serve Machine Learning Model Through ASP.NET Core Web API</span></span>

<span data-ttu-id="09edc-104">En esta guía se muestra cómo publicar un modelo de Machine Learning de ML.NET precompilado en la Web a través de una ASP.NET Core Web API.</span><span class="sxs-lookup"><span data-stu-id="09edc-104">This how-to shows how to serve a pre-built ML.NET machine learning model to the web using an ASP.NET Core Web API.</span></span> <span data-ttu-id="09edc-105">De esta forma se permite que los usuarios accedan a la API para obtener predicciones a través de métodos HTTP estándar.</span><span class="sxs-lookup"><span data-stu-id="09edc-105">By doing so it allows for users to access the API for prediction purposes via standard HTTP methods.</span></span>

> [!NOTE]
> <span data-ttu-id="09edc-106">Este tema hace referencia a ML.NET, que se encuentra actualmente en versión preliminar, por lo que el material está sujeto a cambios.</span><span class="sxs-lookup"><span data-stu-id="09edc-106">This topic refers to ML.NET, which is currently in Preview, and material may be subject to change.</span></span> <span data-ttu-id="09edc-107">Para obtener más información, visite [la introducción de ML.NET](https://www.microsoft.com/net/learn/apps/machine-learning-and-ai/ml-dotnet).</span><span class="sxs-lookup"><span data-stu-id="09edc-107">For more information, visit [the ML.NET introduction](https://www.microsoft.com/net/learn/apps/machine-learning-and-ai/ml-dotnet).</span></span>

<span data-ttu-id="09edc-108">Este tutorial y el ejemplo relacionado usan actualmente **ML.NET en su versión 0.10**.</span><span class="sxs-lookup"><span data-stu-id="09edc-108">This how-to and related sample are currently using **ML.NET version 0.10**.</span></span> <span data-ttu-id="09edc-109">Para obtener más información, consulte las notas de la versión en el [repositorio de GitHub dotnet/machinelearning](https://github.com/dotnet/machinelearning/tree/master/docs/release-notes).</span><span class="sxs-lookup"><span data-stu-id="09edc-109">For more information, see the release notes at the [dotnet/machinelearning github repo](https://github.com/dotnet/machinelearning/tree/master/docs/release-notes).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="09edc-110">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="09edc-110">Prerequisites</span></span>

- <span data-ttu-id="09edc-111">[Visual Studio 2017 15.6 o posterior](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) con la carga de trabajo "Desarrollo multiplataforma de .NET Core" instalada.</span><span class="sxs-lookup"><span data-stu-id="09edc-111">[Visual Studio 2017 15.6 or later](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) with the ".NET Core cross-platform development" workload installed.</span></span>
- <span data-ttu-id="09edc-112">PowerShell.</span><span class="sxs-lookup"><span data-stu-id="09edc-112">Powershell.</span></span>
- <span data-ttu-id="09edc-113">Modelo previamente entrenado.</span><span class="sxs-lookup"><span data-stu-id="09edc-113">Pre-trained model.</span></span>
    - <span data-ttu-id="09edc-114">Use el [tutorial de Análisis de sentimiento de ML.NET](../tutorials/sentiment-analysis.md) para compilar su propio modelo.</span><span class="sxs-lookup"><span data-stu-id="09edc-114">Use the [ML.NET Sentiment Analysis tutorial](../tutorials/sentiment-analysis.md) to build your own model.</span></span>
    - <span data-ttu-id="09edc-115">Descargue este [modelo de Machine Learning de Análisis de sentimiento previamente entrenado](https://github.com/dotnet/samples/blob/master/machine-learning/models/sentimentanalysis/sentiment_model.zip).</span><span class="sxs-lookup"><span data-stu-id="09edc-115">Download this [pre-trained sentiment analysis machine learning model](https://github.com/dotnet/samples/blob/master/machine-learning/models/sentimentanalysis/sentiment_model.zip)</span></span>

## <a name="create-aspnet-core-web-api-project"></a><span data-ttu-id="09edc-116">Creación de un proyecto de ASP.NET Core Web API</span><span class="sxs-lookup"><span data-stu-id="09edc-116">Create ASP.NET Core Web API Project</span></span>

1. <span data-ttu-id="09edc-117">Abra Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="09edc-117">Open Visual Studio 2017.</span></span> <span data-ttu-id="09edc-118">Seleccione **Archivo > Nuevo > Proyecto** en la barra de menús.</span><span class="sxs-lookup"><span data-stu-id="09edc-118">Select **File > New > Project** from the menu bar.</span></span> <span data-ttu-id="09edc-119">En el cuadro de diálogo Nuevo proyecto, seleccione el nodo **Visual C#** seguido del nodo **Web**.</span><span class="sxs-lookup"><span data-stu-id="09edc-119">In the New Project dialog, select the **Visual C#** node followed by the **Web** node.</span></span> <span data-ttu-id="09edc-120">Seleccione la plantilla de proyecto **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="09edc-120">Then select the **ASP.NET Core Web Application** project template.</span></span> <span data-ttu-id="09edc-121">En el cuadro de texto **Nombre**, escriba "SentimentAnalysisWebAPI" y haga clic en el botón **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="09edc-121">In the **Name** text box, type "SentimentAnalysisWebAPI" and then select the **OK** button.</span></span>
1. <span data-ttu-id="09edc-122">En la ventana que muestra los distintos tipos de proyectos de ASP.NET Core, seleccione **API** y haga clic en el botón **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="09edc-122">In the window that displays the different types of ASP.NET Core Projects, select **API** and the select the **OK** button.</span></span>
1. <span data-ttu-id="09edc-123">Cree un directorio denominado *MLModels* en el proyecto para guardar los archivos del modelo de Machine Learning precompilado:</span><span class="sxs-lookup"><span data-stu-id="09edc-123">Create a directory named *MLModels* in your project to save your pre-built machine learning model files:</span></span>

    <span data-ttu-id="09edc-124">En el Explorador de soluciones, haga clic con el botón derecho en el proyecto y seleccione Agregar > Nueva carpeta.</span><span class="sxs-lookup"><span data-stu-id="09edc-124">In Solution Explorer, right-click on your project and select Add > New Folder.</span></span> <span data-ttu-id="09edc-125">Escriba "MLModels" y presione ENTRAR.</span><span class="sxs-lookup"><span data-stu-id="09edc-125">Type "MLModels" and hit Enter.</span></span>

1. <span data-ttu-id="09edc-126">Instale el **paquete NuGet Microsoft.ML**:</span><span class="sxs-lookup"><span data-stu-id="09edc-126">Install the **Microsoft.ML NuGet Package**:</span></span>

    <span data-ttu-id="09edc-127">En el Explorador de soluciones, haga clic con el botón derecho en **Administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="09edc-127">In Solution Explorer, right-click on your project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="09edc-128">Elija "nuget.org" como origen del paquete, seleccione la pestaña Examinar, busque **Microsoft.ML**, seleccione ese paquete en la lista y haga clic en el botón Instalar.</span><span class="sxs-lookup"><span data-stu-id="09edc-128">Choose "nuget.org" as the Package source, select the Browse tab, search for **Microsoft.ML**, select that package in the list, and select the Install button.</span></span> <span data-ttu-id="09edc-129">Seleccione el botón **Aceptar** en el cuadro de diálogo **Vista previa de cambios** y, a continuación, seleccione el botón **Acepto** del cuadro de diálogo Aceptación de la licencia en caso de que esté de acuerdo con los términos de licencia de los paquetes mostrados.</span><span class="sxs-lookup"><span data-stu-id="09edc-129">Select the **OK** button on the **Preview Changes** dialog and then select the **I Accept** button on the License Acceptance dialog if you agree with the license terms for the packages listed.</span></span>

### <a name="add-model-to-aspnet-core-web-api-project"></a><span data-ttu-id="09edc-130">Incorporación del modelo a un proyecto de ASP.NET Core Web API</span><span class="sxs-lookup"><span data-stu-id="09edc-130">Add Model to ASP.NET Core Web API Project</span></span>

1. <span data-ttu-id="09edc-131">Copie el modelo precompilado al directorio *MLModels*.</span><span class="sxs-lookup"><span data-stu-id="09edc-131">Copy your pre-built model to the *MLModels* directory</span></span>
1. <span data-ttu-id="09edc-132">En el Explorador de soluciones, haga clic con el botón derecho en el archivo ZIP del modelo y seleccione Propiedades.</span><span class="sxs-lookup"><span data-stu-id="09edc-132">In Solution Explorer, right-click the model zip file and select Properties.</span></span> <span data-ttu-id="09edc-133">En Avanzadas, cambie el valor de Copiar en el directorio de salida por Copiar si es posterior.</span><span class="sxs-lookup"><span data-stu-id="09edc-133">Under Advanced, change the value of Copy to Output Directory to Copy if newer.</span></span>

## <a name="build-data-models"></a><span data-ttu-id="09edc-134">Creación de modelos de datos</span><span class="sxs-lookup"><span data-stu-id="09edc-134">Build Data Models</span></span>

<span data-ttu-id="09edc-135">Debe crear algunas clases para los datos de entrada y las predicciones.</span><span class="sxs-lookup"><span data-stu-id="09edc-135">You need to create some classes for your input data and predictions.</span></span> <span data-ttu-id="09edc-136">Agregue una nueva clase a su proyecto:</span><span class="sxs-lookup"><span data-stu-id="09edc-136">Add a new class to your project:</span></span>

1. <span data-ttu-id="09edc-137">Cree un directorio denominado *DataModels* en el proyecto para guardar los modelos de datos:</span><span class="sxs-lookup"><span data-stu-id="09edc-137">Create a directory named *DataModels* in your project to save your data models:</span></span>

    <span data-ttu-id="09edc-138">En el Explorador de soluciones, haga clic con el botón derecho en el proyecto y seleccione Agregar > Nueva carpeta.</span><span class="sxs-lookup"><span data-stu-id="09edc-138">In Solution Explorer, right-click on your project and select Add > New Folder.</span></span> <span data-ttu-id="09edc-139">Escriba "DataModels" y presione **ENTRAR**.</span><span class="sxs-lookup"><span data-stu-id="09edc-139">Type "DataModels" and hit **Enter**.</span></span>

2. <span data-ttu-id="09edc-140">En el Explorador de soluciones, haga clic con el botón derecho en el directorio *DataModels* y luego seleccione Agregar > Nuevo elemento.</span><span class="sxs-lookup"><span data-stu-id="09edc-140">In Solution Explorer, right-click the *DataModels* directory, and then select Add > New Item.</span></span>
3. <span data-ttu-id="09edc-141">En el cuadro de diálogo **Agregar nuevo elemento**, seleccione **Clase** y cambie el campo **Nombre** a *SentimentData.cs*.</span><span class="sxs-lookup"><span data-stu-id="09edc-141">In the **Add New Item** dialog box, select **Class** and change the **Name** field to *SentimentData.cs*.</span></span> <span data-ttu-id="09edc-142">A continuación, seleccione el botón **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="09edc-142">Then, select the **Add** button.</span></span> <span data-ttu-id="09edc-143">Se abre el archivo *SentimentData.cs* en el editor de código.</span><span class="sxs-lookup"><span data-stu-id="09edc-143">The *SentimentData.cs* file opens in the code editor.</span></span> <span data-ttu-id="09edc-144">Agregue la instrucción using siguiente en la parte superior del archivo *SentimentData.cs*:</span><span class="sxs-lookup"><span data-stu-id="09edc-144">Add the following using statement to the top of *SentimentData.cs*:</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.ML.Data;
```

<span data-ttu-id="09edc-145">Quite la definición de clase existente y agregue el código siguiente al archivo **SentimentData.cs**:</span><span class="sxs-lookup"><span data-stu-id="09edc-145">Remove the existing class definition and add the following code to the **SentimentData.cs** file:</span></span>

```csharp
public class SentimentData
{
    [LoadColumn(0)]
    public bool Label { get; set; }
    [LoadColumn(1)]
    public string Text { get; set; }   
}
```

4. <span data-ttu-id="09edc-146">En el Explorador de soluciones, haga clic con el botón derecho en el directorio *DataModels* y luego seleccione **Agregar > Nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="09edc-146">In Solution Explorer, right-click the *DataModels* directory, and then select **Add > New Item**.</span></span>
5. <span data-ttu-id="09edc-147">En el cuadro de diálogo **Agregar nuevo elemento**, seleccione **Clase** y cambie el campo **Nombre** a *SentimentPrediction.cs*.</span><span class="sxs-lookup"><span data-stu-id="09edc-147">In the **Add New Item** dialog box, select **Class** and change the **Name** field to *SentimentPrediction.cs*.</span></span> <span data-ttu-id="09edc-148">Luego, haga clic en el botón Agregar.</span><span class="sxs-lookup"><span data-stu-id="09edc-148">Then, select the Add button.</span></span> <span data-ttu-id="09edc-149">El archivo *SentimentPrediction.cs* se abre en el editor de código.</span><span class="sxs-lookup"><span data-stu-id="09edc-149">The *SentimentPrediction.cs* file opens in the code editor.</span></span> <span data-ttu-id="09edc-150">Agregue la instrucción using siguiente en la parte superior del archivo *SentimentPrediction.cs*:</span><span class="sxs-lookup"><span data-stu-id="09edc-150">Add the following using statement to the top of *SentimentPrediction.cs*:</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.ML.Data;
```

<span data-ttu-id="09edc-151">Quite la definición de clase existente y agregue el código siguiente al archivo *SentimentPrediction.cs*:</span><span class="sxs-lookup"><span data-stu-id="09edc-151">Remove the existing class definition and add the following code to the *SentimentPrediction.cs* file:</span></span>

```csharp
public class SentimentPrediction
{
    [ColumnName("PredictedLabel")]
    public bool Prediction { get; set; }
}
```

## <a name="create-prediction-service"></a><span data-ttu-id="09edc-152">Creación del servicio de predicción</span><span class="sxs-lookup"><span data-stu-id="09edc-152">Create Prediction Service</span></span>

<span data-ttu-id="09edc-153">Para organizar y volver a usar la lógica de predicción en toda la aplicación, cree un servicio de predicción.</span><span class="sxs-lookup"><span data-stu-id="09edc-153">To organize and re-use the prediction logic throughout the entire application, create a prediction service.</span></span>

1. <span data-ttu-id="09edc-154">Cree un directorio denominado *Services* en el proyecto para contener servicios que usará la aplicación:</span><span class="sxs-lookup"><span data-stu-id="09edc-154">Create a directory named *Services* in your project to hold services to be used by the application:</span></span>

    <span data-ttu-id="09edc-155">En el Explorador de soluciones, haga clic con el botón derecho en el proyecto y seleccione **Agregar > Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="09edc-155">In Solution Explorer, right-click on your project and select **Add > New Folder**.</span></span> <span data-ttu-id="09edc-156">Escriba "Servicios" y presione **ENTRAR**.</span><span class="sxs-lookup"><span data-stu-id="09edc-156">Type "Services" and hit **Enter**.</span></span>

1. <span data-ttu-id="09edc-157">En el Explorador de soluciones, haga clic con el botón derecho en el directorio *Servicios* y luego seleccione **Agregar > Nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="09edc-157">In Solution Explorer, right-click the *Services* directory, and then select **Add > New Item**.</span></span>
1. <span data-ttu-id="09edc-158">En el cuadro de diálogo **Agregar nuevo elemento**, seleccione **Clase** y cambie el campo **Nombre** a *PredictionService.cs*.</span><span class="sxs-lookup"><span data-stu-id="09edc-158">In the **Add New Item** dialog box, select **Class** and change the **Name** field to *PredictionService.cs*.</span></span> <span data-ttu-id="09edc-159">A continuación, seleccione el botón **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="09edc-159">Then, select the **Add** button.</span></span> <span data-ttu-id="09edc-160">El archivo *PredictionService.cs* se abre en el editor de código.</span><span class="sxs-lookup"><span data-stu-id="09edc-160">The *PredictionService.cs* file opens in the code editor.</span></span> <span data-ttu-id="09edc-161">Agregue la instrucción using siguiente en la parte superior del archivo *PredictionService.cs*:</span><span class="sxs-lookup"><span data-stu-id="09edc-161">Add the following using statement to the top of *PredictionService.cs*:</span></span>

```csharp
using System;
using System.IO;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.ML;
using Microsoft.ML.Core.Data;
using SentimentAnalysisWebAPI.DataModels;
```

<span data-ttu-id="09edc-162">Quite la definición de clase existente y agregue el código siguiente al archivo *PredictionService.cs*:</span><span class="sxs-lookup"><span data-stu-id="09edc-162">Remove the existing class definition and add the following code to the *PredictionService.cs* file:</span></span>

```csharp
public class PredictionService
{
    private readonly PredictionEngine<SentimentData, SentimentPrediction> _predictionEngine;
    public PredictionService(PredictionEngine<SentimentData,SentimentPrediction> predictionEngine)
    {
        _predictionEngine = predictionEngine;
    }

    public string Predict(SentimentData input)
    {
        // Make a prediction
        SentimentPrediction prediction = _predictionEngine.Predict(input);

        //If prediction is true then it is toxic. If it is false, the it is not.
        string isToxic = Convert.ToBoolean(prediction.Prediction) ? "Toxic" : "Not Toxic";

        return isToxic;

    }
}
```

## <a name="register-predictions-service-for-use-in-application"></a><span data-ttu-id="09edc-163">Registro del servicio de predicciones para usarlo en una aplicación</span><span class="sxs-lookup"><span data-stu-id="09edc-163">Register Predictions Service for Use in Application</span></span>

<span data-ttu-id="09edc-164">Para usar el servicio de predicciones en la aplicación, deberá crearlo cada vez que sea necesario.</span><span class="sxs-lookup"><span data-stu-id="09edc-164">To use the prediction service in your application you will have to create it every time it is needed.</span></span> <span data-ttu-id="09edc-165">En ese caso, un procedimiento recomendado que se debe considerar es la inserción de dependencias de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="09edc-165">In that case, a best practice to consider is ASP.NET Core dependency injection.</span></span>

<span data-ttu-id="09edc-166">En el vínculo siguiente se proporciona más información si quiere aprender sobre la [inserción de dependencias](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="09edc-166">The following link provides more information if you want to learn about [dependency injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection?view=aspnetcore-2.1).</span></span>

1. <span data-ttu-id="09edc-167">Abra la clase *Startup.cs* y agregue la siguiente instrucción using en la parte superior del archivo:</span><span class="sxs-lookup"><span data-stu-id="09edc-167">Open the *Startup.cs* class and add the following using statement to the top of the file:</span></span>

```csharp
using System.IO;
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.ML;
using Microsoft.ML.Core.Data;
using SentimentAnalysisWebAPI.DataModels;
using SentimentAnalysisWebAPI.Services;
```

1. <span data-ttu-id="09edc-168">Agregue las líneas de código siguientes al método *ConfigureServices*:</span><span class="sxs-lookup"><span data-stu-id="09edc-168">Add the following lines of code to the *ConfigureServices* method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);

    services.AddSingleton<MLContext>();
    services.AddSingleton<PredictionEngine<SentimentData, SentimentPrediction>>((ctx) =>
    {
        MLContext mlContext = ctx.GetRequiredService<MLContext>();
        string modelFilePathName = "MLModels/sentiment_model.zip";

        //Load model from file
        ITransformer model;
        using (var stream = File.OpenRead(modelFilePathName))
        {
            model = mlContext.Model.Load(stream);
        }

        // Return prediction engine
        return model.CreatePredictionEngine<SentimentData, SentimentPrediction>(mlContext);
    });
    services.AddSingleton<PredictionService>();
}
```

<span data-ttu-id="09edc-169">En un nivel alto, este código inicializa los objetos y servicios de manera automática cuando lo solicita la aplicación en lugar de tener que hacerlo manualmente.</span><span class="sxs-lookup"><span data-stu-id="09edc-169">At a high level, this code initializes the objects and services automatically when requested by the application instead of having to manually do it.</span></span>

## <a name="create-predict-controller"></a><span data-ttu-id="09edc-170">Creación de controlador de predicción</span><span class="sxs-lookup"><span data-stu-id="09edc-170">Create Predict Controller</span></span>

<span data-ttu-id="09edc-171">Para procesar las solicitudes HTTP entrantes, necesita crear un controlador.</span><span class="sxs-lookup"><span data-stu-id="09edc-171">To process your incoming HTTP requests, you need to create a controller.</span></span>

1. <span data-ttu-id="09edc-172">En el Explorador de soluciones, haga clic con el botón derecho en el directorio *Controladores* y seleccione **Agregar > Controlador**.</span><span class="sxs-lookup"><span data-stu-id="09edc-172">In Solution Explorer, right-click the *Controllers* directory, and then select **Add > Controller**.</span></span>
1. <span data-ttu-id="09edc-173">En el cuadro de diálogo **Agregar nuevo elemento**, seleccione **API Controller Empty** (Controlador de API vacío) y seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="09edc-173">In the **Add New Item** dialog box, select **API Controller Empty** and select **Add**.</span></span>
1. <span data-ttu-id="09edc-174">En el símbolo del sistema, cambie el campo **Nombre del controlador** a *PredictController.cs*.</span><span class="sxs-lookup"><span data-stu-id="09edc-174">In the prompt change the **Controller Name** field to *PredictController.cs*.</span></span> <span data-ttu-id="09edc-175">Luego, haga clic en el botón Agregar.</span><span class="sxs-lookup"><span data-stu-id="09edc-175">Then, select the Add button.</span></span> <span data-ttu-id="09edc-176">El archivo *PredictController.cs* se abre en el editor de código.</span><span class="sxs-lookup"><span data-stu-id="09edc-176">The *PredictController.cs* file opens in the code editor.</span></span> <span data-ttu-id="09edc-177">Agregue la instrucción using siguiente en la parte superior del archivo *PredictController.cs*:</span><span class="sxs-lookup"><span data-stu-id="09edc-177">Add the following using statement to the top of *PredictController.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Mvc;
using SentimentAnalysisWebAPI.DataModels;
using SentimentAnalysisWebAPI.Services;
```

<span data-ttu-id="09edc-178">Quite la definición de clase existente y agregue el código siguiente al archivo *PredictController.cs*:</span><span class="sxs-lookup"><span data-stu-id="09edc-178">Remove the existing class definition and add the following code to the *PredictController.cs* file:</span></span>

```csharp
public class PredictController : ControllerBase
{

    private readonly PredictionService _predictionService;

    public PredictController(PredictionService predictionService)
    {
        _predictionService = predictionService; //Define prediction service
    }

    [HttpPost]
    public ActionResult<string> Post([FromBody]SentimentData input)
    {
        if(!ModelState.IsValid)
        {
            return BadRequest();
        }
        return Ok(_predictionService.Predict(input));
    }

}
```

<span data-ttu-id="09edc-179">Esto asigna el servicio de predicción al pasarlo al constructor del controlador que se obtiene a través de una inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="09edc-179">This is assigning the Prediction service by passing it to the controller's constructor which you get via dependency injection.</span></span> <span data-ttu-id="09edc-180">A continuación, en el método POST de este controlador, el servicio de predicción se usa para hacer predicciones y devolver los resultados al usuario si se realiza correctamente.</span><span class="sxs-lookup"><span data-stu-id="09edc-180">Then, in the POST method of this controller the Prediction service is being used to make predictions and return the results back to the user if successful.</span></span>

## <a name="test-web-api-locally"></a><span data-ttu-id="09edc-181">Prueba local de Web API</span><span class="sxs-lookup"><span data-stu-id="09edc-181">Test Web API Locally</span></span>

<span data-ttu-id="09edc-182">Una vez que todo está configurado, es momento de probar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="09edc-182">Once everything is set up, it's time to test the application.</span></span>

1. <span data-ttu-id="09edc-183">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="09edc-183">Run the application.</span></span>
1. <span data-ttu-id="09edc-184">Abra PowerShell y escriba el código siguiente, donde PORT es el puerto donde escuchar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="09edc-184">Open Powershell and enter the following code where PORT is the port your application is listening on.</span></span>

```powershell
Invoke-RestMethod "https://localhost:<PORT>/api/predict" -Method Post -Body (@{Text="This is a very rude movie"} | ConvertTo-Json) -ContentType "application/json"
```

<span data-ttu-id="09edc-185">Si se realiza correctamente, la salida debería ser similar al texto siguiente:</span><span class="sxs-lookup"><span data-stu-id="09edc-185">If successful, the output should look similar to the text below:</span></span>

```powershell
Toxic
```

<span data-ttu-id="09edc-186">¡Enhorabuena!</span><span class="sxs-lookup"><span data-stu-id="09edc-186">Congratulations!</span></span> <span data-ttu-id="09edc-187">Publicó correctamente el modelo para realizar predicciones en Internet mediante una ASP.NET Core API.</span><span class="sxs-lookup"><span data-stu-id="09edc-187">You have successfully served your model to make predictions over the internet using an ASP.NET Core API.</span></span>

## <a name="next-steps"></a><span data-ttu-id="09edc-188">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="09edc-188">Next Steps</span></span>

- [<span data-ttu-id="09edc-189">Implementación en Azure</span><span class="sxs-lookup"><span data-stu-id="09edc-189">Deploy to Azure</span></span>](/aspnet/core/tutorials/publish-to-azure-webapp-using-vs?view=aspnetcore-2.1#deploy-the-app-to-azure)