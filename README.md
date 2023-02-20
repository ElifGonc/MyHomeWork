# MyHomeWork
First Try
*this is my first try*
import cv2 as cv
import numpy as np

image = cv.imread("yuvarlak2.jpg")
gray = cv.cvtColor(image, cv.COLOR_BGR2GRAY)

blur = cv.GaussianBlur(gray, (5, 5),
                       cv.BORDER_DEFAULT)
ret, thresh = cv.threshold(blur, 200, 255,
                           cv.THRESH_BINARY_INV)
cv.imwrite("thresh.png",thresh)

contours, hierarchies = cv.findContours(
    thresh, cv.RETR_LIST, cv.CHAIN_APPROX_SIMPLE)
blank = np.zeros(thresh.shape[:2],
                 dtype='uint8')

cv.drawContours(blank, contours, -1,
                (255, 0, 0), 1)

cv.imwrite("Contours.png", blank)
for i in contours:
    M = cv.moments(i)
    if M['m00'] != 0:
        cx = int(M['m10']/M['m00'])
        cy = int(M['m01']/M['m00'])
        cv.drawContours(image, [i], -1, (0, 255, 0), 2)
        cv.circle(image, (cx, cy), 7, (0, 0, 255), -1)
        cv.putText(image, "center", (cx - 20, cy - 20),
                   cv.FONT_HERSHEY_SIMPLEX, 0.5, (0, 0, 0), 2)
    print(f"x: {cx} y: {cy}")
    cv.imwrite("image.png", image)

    cv.waitKey(0)
    cv.destroyAllWindows()