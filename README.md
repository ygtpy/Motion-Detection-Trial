# Motion Detection in Video

This project implements a basic motion detection algorithm using OpenCV in Python. It processes a video to detect motion and highlights the areas where motion is detected by drawing rectangles around the moving objects.

## Prerequisites

To run this code, you need to have the following installed:
- Python 3.x
- OpenCV (cv2)
- Numpy

You can install the required packages using pip:

```bash
pip install opencv-python numpy
```
# Videoda Hareket Algılama

Bu proje, Python'da OpenCV kullanarak temel bir hareket algılama algoritması uygular. Bir videoyu işleyerek hareketi algılar ve hareket algılandığında hareket eden nesnelerin etrafını dikdörtgenlerle çizer.

## Gereksinimler

Bu kodu çalıştırmak için aşağıdaki yazılımların yüklü olması gerekmektedir:
- Python 3.x
- OpenCV (cv2)
- Numpy

Gerekli paketleri pip kullanarak yükleyebilirsiniz:

```bash
pip install opencv-python numpy
```
## How It Works
The code captures frames from a video file, calculates the difference between consecutive frames, and processes the difference to detect motion. When motion is detected, a rectangle is drawn around the moving area, and a message "Durum: Haraket Algilandi" (which means "Status: Movement Detected" in Turkish) is displayed.

1. **Capture Video:** *The video is loaded using cv2.VideoCapture*
2. **Frame Processing:** *Consecutive frames are read, and the absolute difference between the frames is calculated.*
3. **Grayscale Conversion:** *The difference image is converted to grayscale.*
4. **Blurring:** *The grayscale image is blurred to reduce noise.*
5. Thresholding:** *A binary threshold is applied to highlight the regions with significant differences.*
6. **Dilation:** *The thresholded image is dilated to fill in the gaps.*
7. **Contour Detection:** *Contours are detected in the dilated image.*
8. **Drawing Rectangles:** *Rectangles are drawn around contours with an area larger than a specified threshold.*
9. **Resizing and Writing Output:** *The processed frame is resized and written to an output video file.*

## Nasıl Çalışır
Kod, bir video dosyasından kareler yakalar, ardışık kareler arasındaki farkı hesaplar ve hareketi algılamak için bu farkı işler. Hareket algılandığında, hareket eden alanın etrafına bir dikdörtgen çizilir ve "Durum: Hareket Algılandı" mesajı görüntülenir.

1. **Videoyu Yakalama:** *Video cv2.VideoCapture kullanılarak yüklenir.*
2. **Kare İşleme:** *Ardışık kareler okunur ve kareler arasındaki mutlak fark hesaplanır.*
3. **Gri Tonlamaya Dönüştürme:** *Fark görüntüsü gri tonlamaya dönüştürülür.*
4. **Bulanıklaştırma:** *Gürültüyü azaltmak için gri tonlama görüntüsü bulanıklaştırılır.*
5. **Eşikleme:** *Belirgin farkları vurgulamak için ikili eşikleme uygulanır.*
6. **Genişletme**: *Eşiklenmiş görüntü genişletilerek boşluklar doldurulur.*
7. **Kontur Algılama:** *Genişletilmiş görüntüde konturlar algılanır.*
8. **Dikdörtgen Çizme:** *Belirli bir alanın üzerindeki konturların etrafına dikdörtgenler çizilir.*
9. **Yeniden Boyutlandırma ve Çıktı Yazma:** *İşlenmiş kare yeniden boyutlandırılır ve çıkış video dosyasına yazılır.*

```python
import cv2
import numpy as np

cap = cv2.VideoCapture("redline.mp4")  # Load the video file
frame_width = int(cap.get(cv2.CAP_PROP_FRAME_WIDTH))
frame_height = int(cap.get(cv2.CAP_PROP_FRAME_HEIGHT))

fourcc = cv2.VideoWriter_fourcc('X', 'V', 'I', 'D')  # Define the codec

# Define the output video file
out = cv2.VideoWriter("output222.avi", fourcc, 5.0, (1280, 720))

# Read the first two frames
ret, frame1 = cap.read()
ret, frame2 = cap.read()

while cap.isOpened():
    diff = cv2.absdiff(frame1, frame2)  # Compute the absolute difference
    gray = cv2.cvtColor(diff, cv2.COLOR_BGR2GRAY)  # Convert to grayscale
    blur = cv2.GaussianBlur(gray, (5, 5), 0)  # Apply Gaussian blur
    _, thresh = cv2.threshold(blur, 20, 255, cv2.THRESH_BINARY)  # Apply threshold
    dilated = cv2.dilate(thresh, None, iterations=3)  # Dilate the image
    contours, _ = cv2.findContours(dilated, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)  # Find contours

    for contour in contours:
        (x, y, w, h) = cv2.boundingRect(contour)

        if cv2.contourArea(contour) < 900:
            continue  # Ignore small contours

        # Draw rectangle around the contour
        cv2.rectangle(frame1, (x, y), (x+w, y+h), (0, 255, 0), 2)
        # Display the status message
        cv2.putText(frame1, "Durum: {}".format('Haraket Algilandi'), (10, 20), cv2.FONT_HERSHEY_SIMPLEX,
                    1, (0, 0, 255), 3)
        
    image = cv2.resize(frame1, (1280, 720))  # Resize the frame
    out.write(image)  # Write the frame to the output video file
    cv2.imshow("feed", frame1)  # Display the frame
    frame1 = frame2  # Move to the next frame
    ret, frame2 = cap.read()  # Read the next frame

cv2.destroyAllWindows()
cap.release()
out.release()
```

## Usage
+ *To run the script, execute the following command:*
+ *Scripti çalıştırmak için aşağıdaki komutu çalıştırın:*
```
python trial.py
```
+ **Make sure to replace "redline.mp4" with the path to your input video file.**
+ **redline.mp4" yolunu giriş video dosyanızın yolu ile değiştirmeyi unutmayın.**

## Output
+ *The output video will be saved as output222.avi with motion highlighted by rectangles.*
+ *Hareketin çıktı videosu output222.avi olarak kaydedilecektir.*

