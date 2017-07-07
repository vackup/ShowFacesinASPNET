# ShowFacesinASPNET: Finding the faces in images in ASP.NET web application
From https://code.msdn.microsoft.com/Finding-the-faces-in-in-e99bd310

In this ASP.NET web application based project I have shown how you can process the images uploaded by the users to process them and find the faces. After finding the faces, how you can map their locations in the images for the users to see where faces were found. 

# Building the Sample
I used ASP.NET 4 (MVC 5) for this application development process. Visual Studio 2015 Enterprise edition was used. Other editions and versions would also work in this case. 

# Description
The default code that is used in the project is the following, this code does the maximum work, of finding the faces. Rest of the job is done using JavaScript on the client-side.


C#
```csharp
// Face detector helper object 
public class FaceDetector 
{ 
    public static List<Rectangle> DetectFaces(Mat image) 
    { 
        List<Rectangle> faces = new List<Rectangle>(); 
        var facesCascade = HttpContext.Current.Server.MapPath("~/haarcascade_frontalface_default.xml"); 
        using (CascadeClassifier face = new CascadeClassifier(facesCascade)) 
        { 
            using (UMat ugray = new UMat()) 
            { 
                CvInvoke.CvtColor(image, ugray, Emgu.CV.CvEnum.ColorConversion.Bgr2Gray); 
 
                //normalizes brightness and increases contrast of the image 
                CvInvoke.EqualizeHist(ugray, ugray); 
 
                //Detect the faces from the gray scale image and store the locations as rectangle 
                //The first dimensional is the channel 
                //The second dimension is the index of the rectangle in the specific channel 
                Rectangle[] facesDetected = face.DetectMultiScale( 
                                                ugray, 
                                                1.1, 
                                                10, 
                                                new Size(20, 20)); 
 
                faces.AddRange(facesDetected); 
            } 
        } 
        return faces; 
    } 
}
```

Note that the above code snippet requires Emgu CV helpers to be installed before you can call the above code. The rest of the code can be viewed in the sample itself. Download and try it out yourself.

# Source Code Files
* FindFacesController.cs - This file contains the controller and actions code that processes the images on the server-side once the user has uploaded them. It also contains the helper objects such as Location and FaceDetector object.
* Views / FindFaces / Index.cshtml - This file is the view for the project and is used to render the HTML content. It has an HTML form by default and later would show the results of the images that are processed. 

# More Information
For more information and a guide to learn about this, please read the following article of mine, [Highlighting the faces in uploaded image in ASP.NET web applications](https://basicsofwebdevelopment.wordpress.com/2016/08/17/highlighting-the-faces-in-uploaded-image-in-asp-net-web-applications/)
