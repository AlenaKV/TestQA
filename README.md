# TestQA
#### Данный скрипт написан для нахождения центра пятна на изображении, расчета дисперсии, стандартного отклонения и занесения данных метрик в созданную БД.
---
Импорт изображения:

**img = cv2.imread("/content/test_picture.jpg")**

**if img is None:**

**print('Image not found')**

---
Перевод изображения из цветного в бинарное:

**gray_image = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)**

**ret, thresh = cv2.threshold(gray_image, 127, 255, 0)**

---
Расчет моментов и расчет координат центра пятна:

**M = cv2.moments(thresh)**

**centrX = int(M["m10"] / M["m00"])**	

**centrY = int(M["m01"] / M["m00"])**

