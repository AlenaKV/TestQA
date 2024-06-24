# TestQA
#### Данный скрипт написан для нахождения центра пятна на изображении, расчета дисперсии, стандартного отклонения и занесения данных метрик в созданную БД.
---
Импорт изображения:
```
img = cv2.imread("/content/test_picture.jpg")
if img is None:
  print('Image not found')
```

---
Перевод изображения из цветного в бинарное:
```
gray_image = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
ret, thresh = cv2.threshold(gray_image, 127, 255, 0)
```
---
Расчет моментов и расчет координат центра пятна:
```
M = cv2.moments(thresh)
centrX = int(M["m10"] / M["m00"])	
centrY = int(M["m01"] / M["m00"])
```
---
Нахождение контуров на изображении при помощи функции `cv2.findContours` и заполнение массивов с координатами точек границы:
```
contours=cv2.findContours(thresh, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_NONE)
contours_p = np.vstack(contours[0]).squeeze()

x_coordinates = contours_p[:, 0]
y_coordinates = contours_p[:, 1]
```
---
Заполнение массива длин радиусов пятна:
```
for i in range(len(x_coordinates)):
    lenghtvec.append(math.sqrt((x_coordinates[i]-centrX)**2+(y_coordinates[i]-centrY)**2))
```
---
Вывод стандарного отклонения и дисперсии:
```
print('Дисперсия: ', np.var(lenghtvec))
  print('Стандартное отклонение: ' ,np.std(lenghtvec))
```



