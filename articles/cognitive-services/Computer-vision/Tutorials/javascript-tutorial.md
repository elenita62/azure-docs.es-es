---
title: 'Tutorial: Computer Vision API para JavaScript'
titlesuffix: Azure Cognitive Services
description: Explore una aplicación básica de JavaScript que usa Computer Vision API en Azure Cognitive Services. Realice OCR, cree miniaturas y trabaje con características visuales en una imagen.
services: cognitive-services
author: KellyDF
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: tutorial
ms.date: 09/19/2017
ms.author: kefre
ms.openlocfilehash: c024e517eb59c7d3b61408e477c94004ccb01a54
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "49341317"
---
# <a name="tutorial-computer-vision-api-javascript"></a>Tutorial: Computer Vision API para JavaScript

En este tutorial se muestran las características de la API de REST Computer Vision de Azure Cognitive Services.

Explore una aplicación de JavaScript que usa la API REST Computer Vision API para realizar el reconocimiento óptico de caracteres (OCR), crear miniaturas con recorte inteligente, y detectar, clasificar, etiquetar y describir características visuales, como caras, en una imagen. Este ejemplo le permite enviar la dirección URL de una imagen para su análisis o procesamiento. Puede usar este ejemplo de código abierto como plantilla para crear su propia aplicación de JavaScript para usar la API REST de Computer Vision.

La aplicación de JavaScript ya se ha escrito, pero no tiene ninguna funcionalidad de Computer Vision. En este tutorial, agregará el código específico de la API REST Computer Vision API para completar la funcionalidad de la aplicación.

## <a name="prerequisites"></a>Requisitos previos

### <a name="platform-requirements"></a>Requisitos de la plataforma

Este tutorial se ha desarrollado con un editor de texto simple.

### <a name="subscribe-to-computer-vision-api-and-get-a-subscription-key"></a>Suscripción a Computer Vision API y obtención de una clave de suscripción 

Antes de crear el ejemplo, debe suscribirse a Computer Vision API, que forma parte de Azure Cognitive Services. Para más información sobre la administración de claves y suscripciones, consulte [Suscripciones](https://azure.microsoft.com/try/cognitive-services/). En este tutorial son válidas las claves principal y secundaria. 

## <a name="acquire-the-incomplete-tutorial-project"></a>Adquisición del proyecto del tutorial incompleto

### <a name="download-the-tutorial-project"></a>Descarga del proyecto del tutorial

Clone el [tutorial de Computer Vision de Cognitive Services para JavaScript](https://github.com/Azure-Samples/cognitive-services-javascript-computer-vision-tutorial) o descargue el archivo .zip y extráigalo en un directorio vacío.

Si prefiere usar el tutorial terminado que ya tiene todo el código agregado, puede usar los archivos de la carpeta **Completed**.

## <a name="add-the-tutorial-code-to-the-project"></a>Adición del código del tutorial al proyecto

La aplicación de JavaScript se instala con seis archivos .html, uno para cada característica. Cada archivo muestra una función diferente de Computer Vision (analizar, OCR, etc.). Las seis secciones del tutorial son independientes entre sí, por lo que puede agregar el código del tutorial a un archivo, los seis archivos o solo un par de ellos. Puede agregar el código del tutorial a los archivos en cualquier orden.

Comencemos.

### <a name="analyze-an-image"></a>Análisis de una imagen

La característica Analyze de Computer Vision examina una imagen en busca de más de 2000 objetos reconocibles, seres vivos, escenarios y acciones. Una vez completado el análisis, Analyze devuelve un objeto JSON que describe la imagen con etiquetas descriptivas, análisis de color o títulos, entre otros.

Para completar la característica Analyze de la aplicación del tutorial, siga estos pasos:

#### <a name="add-the-event-handler-code-for-the-form-button"></a>Adición del código de controlador de eventos para el botón del formulario

Abra el archivo **analyze.html** en un editor de texto y busque la función **analyzeButtonClick** cerca de la parte inferior del archivo.

La función **analyzeButtonClick** del controlador de eventos borra el formulario, muestra la imagen especificada en la dirección URL y llama a la función **AnalyzeImage** para analizar la imagen.

Copie y pegue el código siguiente en la función **analyzeButtonClick**.

```javascript
function analyzeButtonClick() {

    // Clear the display fields.
    $("#sourceImage").attr("src", "#");
    $("#responseTextArea").val("");
    $("#captionSpan").text("");
    
    // Display the image.
    var sourceImageUrl = $("#inputImage").val();
    $("#sourceImage").attr("src", sourceImageUrl);
    
    AnalyzeImage(sourceImageUrl, $("#responseTextArea"), $("#captionSpan"));
}
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>Adición del contenedor para la llamada a API REST

La función **AnalyzeImage** ajusta la llamada API REST para analizar una imagen. Tras una devolución correcta, el análisis JSON con formato se mostrará en el área de texto especificada y el título se mostrará en el intervalo especificado.

Copie y pegue el código de función **AnalyzeImage** en la posición situada justo debajo de la función **analyzeButtonClick**.

```javascript
/* Analyze the image at the specified URL by using Microsoft Cognitive Services Analyze Image API.
 * @param {string} sourceImageUrl - The URL to the image to analyze.
 * @param {<textarea> element} responseTextArea - The text area to display the JSON string returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 * @param {<span> element} captionSpan - The span to display the image caption.
 */
function AnalyzeImage(sourceImageUrl, responseTextArea, captionSpan) {
    // Request parameters.
    var params = {
        "visualFeatures": "Categories,Description,Color",
        "details": "",
        "language": "en",
    };
    
    // Perform the REST API call.
    $.ajax({
        url: common.uriBasePreRegion + 
             $("#subscriptionRegionSelect").val() + 
             common.uriBasePostRegion + 
             common.uriBaseAnalyze +
             "?" + 
             $.param(params),
                    
        // Request headers.
        beforeSend: function(jqXHR){
            jqXHR.setRequestHeader("Content-Type","application/json");
            jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key", 
                encodeURIComponent($("#subscriptionKeyInput").val()));
        },
        
        type: "POST",
        
        // Request body.
        data: '{"url": ' + '"' + sourceImageUrl + '"}',
    })
    
    .done(function(data) {
        // Show formatted JSON on webpage.
        responseTextArea.val(JSON.stringify(data, null, 2));
        
        // Extract and display the caption and confidence from the first caption in the description object.
        if (data.description && data.description.captions) {
            var caption = data.description.captions[0];
            
            if (caption.text && caption.confidence) {
                captionSpan.text("Caption: " + caption.text +
                    " (confidence: " + caption.confidence + ").");
            }
        }
    })
    
    .fail(function(jqXHR, textStatus, errorThrown) {
        // Prepare the error string.
        var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
        errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
            jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
        
        // Put the error JSON in the response textarea.
        responseTextArea.val(JSON.stringify(jqXHR, null, 2));
        
        // Show the error message.
        alert(errorString);
    });
}
```

#### <a name="run-the-application"></a>Ejecución de la aplicación

Guarde el archivo **analyze.html** y ábralo en un explorador web. Coloque la clave de suscripción en el campo **Subscription Key** (Clave de suscripción) y compruebe que está usando la región correcta en **Subscription Region** (Región de suscripción). Escriba una dirección URL a la imagen que desea analizar y haga clic en el botón **Analyze Image** (Analizar imagen) para analizar una imagen y ver el resultado.

### <a name="recognize-a-landmark"></a>Reconocimiento de un lugar de interés

La característica Landmark de Computer Vision analiza una imagen en busca de lugares de interés naturales y artificiales, tales como montañas o edificios famosos. Una vez completado el análisis, Landmark devuelve un objeto JSON que identifica los lugares de interés que encuentre en la imagen.

Para completar la característica Landmark de la aplicación del tutorial, siga estos pasos:

#### <a name="add-the-event-handler-code-for-the-form-button"></a>Adición del código de controlador de eventos para el botón del formulario

Abra el archivo **landmark.html** en un editor de texto y busque la función **landmarkButtonClick** cerca de la parte inferior del archivo.

La función **landmarkButtonClick** del controlador de eventos borra el formulario, muestra la imagen especificada en la dirección URL y llama a la función **IdentifyLandmarks** para analizar la imagen.

Copie y pegue el código siguiente en la función **landmarkButtonClick**.

```javascript
function landmarkButtonClick() {

    // Clear the display fields.
    $("#sourceImage").attr("src", "#");
    $("#responseTextArea").val("");
    $("#captionSpan").text("");
    
    // Display the image.
    var sourceImageUrl = $("#inputImage").val();
    $("#sourceImage").attr("src", sourceImageUrl);
    
    IdentifyLandmarks(sourceImageUrl, $("#responseTextArea"), $("#captionSpan"));
}
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>Adición del contenedor para la llamada a API REST

La función **IdentifyLandmarks** ajusta la llamada API REST para analizar una imagen. Tras una devolución correcta, el análisis JSON con formato se mostrará en el área de texto especificada y el título se mostrará en el intervalo especificado.

Copie y pegue el código de función **IdentifyLandmarks** en la posición situada justo debajo de la función **landmarkButtonClick**.

```javascript
/* Identify landmarks in the image at the specified URL by using Microsoft Cognitive Services 
 * Landmarks API.
 * @param {string} sourceImageUrl - The URL to the image to analyze for landmarks.
 * @param {<textarea> element} responseTextArea - The text area to display the JSON string returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 * @param {<span> element} captionSpan - The span to display the image caption.
 */
function IdentifyLandmarks(sourceImageUrl, responseTextArea, captionSpan) {
    // Request parameters.
    var params = {
        "model": "landmarks"
    };
    
    // Perform the REST API call.
    $.ajax({
        url: common.uriBasePreRegion + 
             $("#subscriptionRegionSelect").val() + 
             common.uriBasePostRegion + 
             common.uriBaseLandmark +
             "?" + 
             $.param(params),
                    
        // Request headers.
        beforeSend: function(jqXHR){
            jqXHR.setRequestHeader("Content-Type","application/json");
            jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key", 
                encodeURIComponent($("#subscriptionKeyInput").val()));
        },
        
        type: "POST",
        
        // Request body.
        data: '{"url": ' + '"' + sourceImageUrl + '"}',
    })
    
    .done(function(data) {
        // Show formatted JSON on webpage.
        responseTextArea.val(JSON.stringify(data, null, 2));
        
        // Extract and display the caption and confidence from the first caption in the description object.
        if (data.result && data.result.landmarks) {
            var landmark = data.result.landmarks[0];
            
            if (landmark.name && landmark.confidence) {
                captionSpan.text("Landmark: " + landmark.name +
                    " (confidence: " + landmark.confidence + ").");
            }
        }
    })
    
    .fail(function(jqXHR, textStatus, errorThrown) {
        // Prepare the error string.
        var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
        errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
            jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
        
        // Put the error JSON in the response textarea.
        responseTextArea.val(JSON.stringify(jqXHR, null, 2));
        
        // Show the error message.
        alert(errorString);
    });
}
```

#### <a name="run-the-application"></a>Ejecución de la aplicación

Guarde el archivo **landmark.html** y ábralo en un explorador web. Coloque la clave de suscripción en el campo **Subscription Key** (Clave de suscripción) y compruebe que está usando la región correcta en **Subscription Region** (Región de suscripción). Escriba una dirección URL a la imagen que desea analizar y haga clic en el botón **Analyze Image** (Analizar imagen) para analizar una imagen y ver el resultado.

### <a name="recognize-celebrities"></a>Reconocimiento de celebridades

La característica Celebrities de Computer Vision analiza una imagen en busca de personas famosas. Una vez completado el análisis, Celebrities devuelve un objeto JSON que identifica las personas famosas que encuentre en la imagen.

Para completar la característica Celebrities de la aplicación del tutorial, siga estos pasos:

#### <a name="add-the-event-handler-code-for-the-form-button"></a>Adición del código de controlador de eventos para el botón del formulario

Abra el archivo **celebrities.html** en un editor de texto y busque la función **celebritiesButtonClick** cerca de la parte inferior del archivo.

La función **celebritiesButtonClick** del controlador de eventos borra el formulario, muestra la imagen especificada en la dirección URL y llama a la función **IdentifyCelebrities** para analizar la imagen.

Copie y pegue el código siguiente en la función **celebritiesButtonClick**.

```javascript
function celebritiesButtonClick() {

    // Clear the display fields.
    $("#sourceImage").attr("src", "#");
    $("#responseTextArea").val("");
    $("#captionSpan").text("");
    
    // Display the image.
    var sourceImageUrl = $("#inputImage").val();
    $("#sourceImage").attr("src", sourceImageUrl);
    
    IdentifyCelebrities(sourceImageUrl, $("#responseTextArea"), $("#captionSpan"));
}
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>Adición del contenedor para la llamada a API REST

```javascript
/* Identify celebrities in the image at the specified URL by using Microsoft Cognitive Services 
 * Celebrities API.
 * @param {string} sourceImageUrl - The URL to the image to analyze for celebrities.
 * @param {<textarea> element} responseTextArea - The text area to display the JSON string returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 * @param {<span> element} captionSpan - The span to display the image caption.
 */
function IdentifyCelebrities(sourceImageUrl, responseTextArea, captionSpan) {
    // Request parameters.
    var params = {
        "model": "celebrities"
    };
    
    // Perform the REST API call.
    $.ajax({
        url: common.uriBasePreRegion + 
             $("#subscriptionRegionSelect").val() + 
             common.uriBasePostRegion + 
             common.uriBaseCelebrities +
             "?" + 
             $.param(params),
                    
        // Request headers.
        beforeSend: function(jqXHR){
            jqXHR.setRequestHeader("Content-Type","application/json");
            jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key", 
                encodeURIComponent($("#subscriptionKeyInput").val()));
        },
        
        type: "POST",
        
        // Request body.
        data: '{"url": ' + '"' + sourceImageUrl + '"}',
    })
    
    .done(function(data) {
        // Show formatted JSON on webpage.
        responseTextArea.val(JSON.stringify(data, null, 2));
        
        // Extract and display the caption and confidence from the first caption in the description object.
        if (data.result && data.result.celebrities) {
            var celebrity = data.result.celebrities[0];
            
            if (celebrity.name && celebrity.confidence) {
                captionSpan.text("Celebrity name: " + celebrity.name +
                    " (confidence: " + celebrity.confidence + ").");
            }
        }
    })
    
    .fail(function(jqXHR, textStatus, errorThrown) {
        // Prepare the error string.
        var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
        errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
            jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
        
        // Put the error JSON in the response textarea.
        responseTextArea.val(JSON.stringify(jqXHR, null, 2));
        
        // Show the error message.
        alert(errorString);
    });
}
```

#### <a name="run-the-application"></a>Ejecución de la aplicación

Guarde el archivo **celebrities.html** y ábralo en un explorador web. Coloque la clave de suscripción en el campo **Subscription Key** (Clave de suscripción) y compruebe que está usando la región correcta en **Subscription Region** (Región de suscripción). Escriba una dirección URL a la imagen que desea analizar y haga clic en el botón **Analyze Image** (Analizar imagen) para analizar una imagen y ver el resultado.

### <a name="intelligently-generate-a-thumbnail"></a>Generación inteligente de una miniatura

La característica Thumbnail de Computer Vision genera una miniatura de una imagen. Con la característica **Smart Crop**, la característica Thumbnail identificará el área de interés de una imagen y centrará la miniatura en esta área para generar miniaturas más bonitas.

Para completar la característica Thumbnail de la aplicación del tutorial, siga estos pasos:

#### <a name="add-the-event-handler-code-for-the-form-button"></a>Adición del código de controlador de eventos para el botón del formulario

Abra el archivo **thumbnail.html** en un editor de texto y busque la función **thumbnailButtonClick** cerca de la parte inferior del archivo.

La función **thumbnailButtonClick** de controlador de eventos borra el formulario, muestra la imagen especificada en la dirección URL y llama a la función **getThumbnail** dos veces para crear dos miniaturas, una con recorte inteligente y otra sin recorte inteligente.

Copie y pegue el código siguiente en la función **thumbnailButtonClick**.

```javascript
function thumbnailButtonClick() {

    // Clear the display fields.
    document.getElementById("sourceImage").src = "#";
    document.getElementById("thumbnailImageSmartCrop").src = "#";
    document.getElementById("thumbnailImageNonSmartCrop").src = "#";
    document.getElementById("responseTextArea").value = "";
    document.getElementById("captionSpan").text = "";
    
    // Display the image.
    var sourceImageUrl = document.getElementById("inputImage").value;
    document.getElementById("sourceImage").src = sourceImageUrl;
    
    // Get a smart cropped thumbnail.
    getThumbnail (sourceImageUrl, true, document.getElementById("thumbnailImageSmartCrop"), 
        document.getElementById("responseTextArea"));
    
    // Get a non-smart-cropped thumbnail.
    getThumbnail (sourceImageUrl, false, document.getElementById("thumbnailImageNonSmartCrop"),
        document.getElementById("responseTextArea"));
}
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>Adición del contenedor para la llamada a API REST

La función **getThumbnail** ajusta la llamada API REST para analizar una imagen. Si la devolución es correcta, la miniatura se mostrará en el elemento img especificado.

Copie y pegue la siguiente función **getThumbnail** debajo de la función **thumbnailButtonClick**.

```javascript
/* Get a thumbnail of the image at the specified URL by using Microsoft Cognitive Services
 * Thumbnail API.
 * @param {string} sourceImageUrl URL to image.
 * @param {boolean} smartCropping Set to true to use the smart cropping feature which crops to the
 *                                more interesting area of an image; false to crop for the center
 *                                of the image.
 * @param {<img> element} imageElement The img element in the DOM which will display the thumnail image.
 * @param {<textarea> element} responseTextArea - The text area to display the Response Headers returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 */
function getThumbnail (sourceImageUrl, smartCropping, imageElement, responseTextArea) {
    // Create the HTTP Request object.
    var xhr = new XMLHttpRequest();
    
    // Request parameters.
    var params = "width=100&height=150&smartCropping=" + smartCropping.toString();

    // Build the full URI.
    var fullUri = common.uriBasePreRegion + 
                  document.getElementById("subscriptionRegionSelect").value + 
                  common.uriBasePostRegion + 
                  common.uriBaseThumbnail +
                  "?" + 
                  params;
    
    // Identify the request as a POST, with the URI and parameters.
    xhr.open("POST", fullUri);
    
    // Add the request headers.
    xhr.setRequestHeader("Content-Type","application/json");
    xhr.setRequestHeader("Ocp-Apim-Subscription-Key", 
        encodeURIComponent(document.getElementById("subscriptionKeyInput").value));
    
    // Set the response type to "blob" for the thumbnail image data.
    xhr.responseType = "blob";
    
    // Process the result of the REST API call.
    xhr.onreadystatechange = function(e) {
        if(xhr.readyState === XMLHttpRequest.DONE) {
            
            // Thumbnail successfully created.
            if (xhr.status === 200) {
                // Show response headers.
                var s = JSON.stringify(xhr.getAllResponseHeaders(), null, 2);
                responseTextArea.value = JSON.stringify(xhr.getAllResponseHeaders(), null, 2);
                
                // Show thumbnail image.
                var urlCreator = window.URL || window.webkitURL;
                var imageUrl = urlCreator.createObjectURL(this.response);
                imageElement.src = imageUrl;
            } else {
                // Display the error message. The error message is the response body as a JSON string. 
                // The code in this code block extracts the JSON string from the blob response.
                var reader = new FileReader();
                
                // This event fires after the blob has been read.
                reader.addEventListener('loadend', (e) => {
                    responseTextArea.value = JSON.stringify(JSON.parse(e.srcElement.result), null, 2);
                });
                
                // Start reading the blob as text.
                reader.readAsText(xhr.response);
            }
        }
    }
    
    // Execute the REST API call.
    xhr.send('{"url": ' + '"' + sourceImageUrl + '"}');
}
```

#### <a name="run-the-application"></a>Ejecución de la aplicación

Guarde el archivo **thumbnail.html** y ábralo en un explorador web. Coloque la clave de suscripción en el campo **Subscription Key** (Clave de suscripción) y compruebe que está usando la región correcta en **Subscription Region** (Región de suscripción). Escriba una dirección URL a la imagen que desea analizar y haga clic en el botón **Generate Thumbnails** (Generar miniaturas) para analizar una imagen y ver el resultado.

### <a name="read-printed-text-ocr"></a>Lectura de texto impreso (OCR)

La característica de reconocimiento óptico de caracteres (OCR) de Computer Vision analiza una imagen de texto impreso. Una vez completado el análisis, OCR devuelve un objeto JSON que contiene el texto y la ubicación del texto de la imagen.

Para completar la característica OCR de la aplicación del tutorial, siga estos pasos:

### <a name="ocr-step-1-add-the-event-handler-code-for-the-form-button"></a>Paso 1 de OCR: Adición del código de controlador de eventos para el botón del formulario

Abra el archivo **ocr.html** en un editor de texto y busque la función **ocrButtonClick** cerca de la parte inferior del archivo.

La función **ocrButtonClick** del controlador de eventos borra el formulario, muestra la imagen especificada en la dirección URL y llama a la función **ReadOcrImage** para analizar la imagen.

Copie y pegue el código siguiente en la función **ocrButtonClick**.

```javascript
function ocrButtonClick() {

    // Clear the display fields.
    $("#sourceImage").attr("src", "#");
    $("#responseTextArea").val("");
    $("#captionSpan").text("");
    
    // Display the image.
    var sourceImageUrl = $("#inputImage").val();
    $("#sourceImage").attr("src", sourceImageUrl);
    
    ReadOcrImage(sourceImageUrl, $("#responseTextArea"));
}
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>Adición del contenedor para la llamada a API REST

La función **ReadOcrImage** ajusta la llamada API REST para analizar una imagen. Una vez devuelto el resultado, el código JSON con formato que describe el texto y la ubicación del texto se muestran en el área de texto especificada.

Copie y pegue la siguiente función **ReadOcrImage** debajo de la función **ocrButtonClick**.

```javascript
/* Recognize and read printed text in an image at the specified URL by using Microsoft Cognitive 
 * Services OCR API.
 * @param {string} sourceImageUrl - The URL to the image to analyze for printed text.
 * @param {<textarea> element} responseTextArea - The text area to display the JSON string returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 */
function ReadOcrImage(sourceImageUrl, responseTextArea) {
    // Request parameters.
    var params = {
        "language": "unk",
        "detectOrientation ": "true",
    };

    // Perform the REST API call.
    $.ajax({
        url: common.uriBasePreRegion + 
             $("#subscriptionRegionSelect").val() + 
             common.uriBasePostRegion + 
             common.uriBaseOcr +
             "?" + 
             $.param(params),
        
        // Request headers.
        beforeSend: function(jqXHR){
            jqXHR.setRequestHeader("Content-Type","application/json");
            jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key", 
                encodeURIComponent($("#subscriptionKeyInput").val()));
        },
        
        type: "POST",
        
        // Request body.
        data: '{"url": ' + '"' + sourceImageUrl + '"}',
    })
    
    .done(function(data) {
        // Show formatted JSON on webpage.
        responseTextArea.val(JSON.stringify(data, null, 2));
    })
    
    .fail(function(jqXHR, textStatus, errorThrown) {
        // Put the JSON description into the text area.
        responseTextArea.val(JSON.stringify(jqXHR, null, 2));
        
        // Display error message.
        var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
        errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
            jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
        alert(errorString);
    });
}
```

#### <a name="run-the-application"></a>Ejecución de la aplicación

Guarde el archivo **ocr.html** y ábralo en un explorador web. Coloque la clave de suscripción en el campo **Subscription Key** (Clave de suscripción) y compruebe que está usando la región correcta en **Subscription Region** (Región de suscripción). Escriba una dirección URL a la imagen que desea leer y haga clic en el botón **Read Image** (Leer imagen) para analizar una imagen y ver el resultado.

### <a name="read-handwritten-text-handwriting-recognition"></a>Lectura de texto escrito a mano (Handwriting Recognition)

La característica Handwriting Recognition de Computer Vision analiza una imagen de texto escrito a mano. Una vez completado el análisis, Handwriting Recognition devuelve un objeto JSON que contiene el texto y la ubicación del texto de la imagen.

Para completar la característica Handwriting Recognition de la aplicación del tutorial, siga estos pasos:

#### <a name="add-the-event-handler-code-for-the-form-button"></a>Adición del código de controlador de eventos para el botón del formulario

Abra el archivo **handwriting.html** en un editor de texto y busque la función **handwritingButtonClick** cerca de la parte inferior del archivo.

La función **handwritingButtonClick** del controlador de eventos borra el formulario, muestra la imagen especificada en la dirección URL y llama a la función **HandwritingImage** para analizar la imagen.

Copie y pegue el código siguiente en la función **handwritingButtonClick**.

```javascript
function handwritingButtonClick() {

    // Clear the display fields.
    $("#sourceImage").attr("src", "#");
    $("#responseTextArea").val("");
    
    // Display the image.
    var sourceImageUrl = $("#inputImage").val();
    $("#sourceImage").attr("src", sourceImageUrl);
    
    ReadHandwrittenImage(sourceImageUrl, $("#responseTextArea"));
}
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>Adición del contenedor para la llamada a API REST

La función **ReadHandwrittenImage** encapsula las dos llamadas API REST necesarias para analizar una imagen. Como el reconocimiento de la escritura a mano es un proceso lento, se utiliza un proceso en dos pasos. La primera llamada envía la imagen para su procesamiento; la segunda llamada recupera el texto detectado una vez completado el procesamiento.

Después de recuperar el texto, el código JSON con formato que describe el texto y la ubicación del texto se muestran en el área de texto especificada.

Copie y pegue la siguiente función **ReadHandwrittenImage** debajo de la función **handwritingButtonClick**.

```javascript
/* Recognize and read text from an image of handwriting at the specified URL by using Microsoft 
 * Cognitive Services Recognize Handwritten Text API.
 * @param {string} sourceImageUrl - The URL to the image to analyze for handwriting.
 * @param {<textarea> element} responseTextArea - The text area to display the JSON string returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 */
function ReadHandwrittenImage(sourceImageUrl, responseTextArea) {
    // Request parameters.
    var params = {
        "handwriting": "true",
    };

    // This operation requrires two REST API calls. One to submit the image for processing,
    // the other to retrieve the text found in the image. 
    //
    // Perform the first REST API call to submit the image for processing.
    $.ajax({
        url: common.uriBasePreRegion + 
             $("#subscriptionRegionSelect").val() + 
             common.uriBasePostRegion + 
             common.uriBaseHandwriting +
             "?" + 
             $.param(params),
        
        // Request headers.
        beforeSend: function(jqXHR){
            jqXHR.setRequestHeader("Content-Type","application/json");
            jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key", 
                encodeURIComponent($("#subscriptionKeyInput").val()));
        },
        
        type: "POST",
        
        // Request body.
        data: '{"url": ' + '"' + sourceImageUrl + '"}',
    })
    
    .done(function(data, textStatus, jqXHR) {
        // Show progress.
        responseTextArea.val("Handwritten image submitted.");
        
        // Note: The response may not be immediately available. Handwriting Recognition is an
        // async operation that can take a variable amount of time depending on the length
        // of the text you want to recognize. You may need to wait or retry this GET operation.
        //
        // Try once per second for up to ten seconds to receive the result.
        var tries = 10;
        var waitTime = 100;
        var taskCompleted = false;
        
        var timeoutID = setInterval(function () { 
            // Limit the number of calls.
            if (--tries <= 0) {
                window.clearTimeout(timeoutID);
                responseTextArea.val("The response was not available in the time allowed.");
                return;
            }

            // The "Operation-Location" in the response contains the URI to retrieve the recognized text.
            var operationLocation = jqXHR.getResponseHeader("Operation-Location");
            
            // Perform the second REST API call and get the response.
            $.ajax({
                url: operationLocation,
                
                // Request headers.
                beforeSend: function(jqXHR){
                    jqXHR.setRequestHeader("Content-Type","application/json");
                    jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key",
                        encodeURIComponent($("#subscriptionKeyInput").val()));
                },
                
                type: "GET",
            })
            
            .done(function(data) {
                // If the result is not yet available, return.
                if (data.status && (data.status === "NotStarted" || data.status === "Running")) {
                    return;
                }
                
                // Show formatted JSON on webpage.
                responseTextArea.val(JSON.stringify(data, null, 2));
                
                // Indicate the task is complete and clear the timer.
                taskCompleted = true;
                window.clearTimeout(timeoutID);
            })
            
            .fail(function(jqXHR, textStatus, errorThrown) {
                // Indicate the task is complete and clear the timer.
                taskCompleted = true;
                window.clearTimeout(timeoutID);
                
                // Display error message.
                var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
                errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
                    jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
                alert(errorString);
            });
        }, waitTime);
    })
    
    .fail(function(jqXHR, textStatus, errorThrown) {
        // Put the JSON description into the text area.
        responseTextArea.val(JSON.stringify(jqXHR, null, 2));
        
        // Display error message.
        var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
        errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
            jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
        alert(errorString);
    });
}
```

#### <a name="run-the-application"></a>Ejecución de la aplicación

Guarde el archivo **handwriting.html** y ábralo en un explorador web. Coloque la clave de suscripción en el campo **Subscription Key** (Clave de suscripción) y compruebe que está usando la región correcta en **Subscription Region** (Región de suscripción). Escriba una dirección URL a la imagen que desea leer y haga clic en el botón **Read Image** (Leer imagen) para analizar una imagen y ver el resultado.

## <a name="next-steps"></a>Pasos siguientes

- [Tutorial de Computer Vision API para C#](CSharpTutorial.md)
- [Tutorial de Computer Vision API para Python](PythonTutorial.md)
