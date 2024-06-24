# TestQA
#### Данный скрипт написан для нахождения центра пятна на изображении, расчета дисперсии, стандартного отклонения и занесения данных метрик в созданную БД.
---
Импорт изображения:
```
img = cv2.imread("/content/test_picture.jpg")
if img is None:
  print('Image not found')
```


Перевод изображения из цветного в бинарное:
```
gray_image = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
ret, thresh = cv2.threshold(gray_image, 127, 255, 0)
```

Расчет моментов и расчет координат центра пятна:
```
M = cv2.moments(thresh)
centrX = int(M["m10"] / M["m00"])	
centrY = int(M["m01"] / M["m00"])
```

Нахождение контуров на изображении при помощи функции `cv2.findContours` и заполнение массивов с координатами точек границы:
```
contours=cv2.findContours(thresh, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_NONE)
contours_p = np.vstack(contours[0]).squeeze()

x_coordinates = contours_p[:, 0]
y_coordinates = contours_p[:, 1]
```

Заполнение массива длин радиусов пятна:
```
for i in range(len(x_coordinates)):
    lenghtvec.append(math.sqrt((x_coordinates[i]-centrX)**2+(y_coordinates[i]-centrY)**2))
```

Отправка стандарного отклонения и дисперсии в базу данных `Metrics`:
```
cursor.execute('INSERT INTO Metrics (variance, standard_deviation) VALUES (?, ?)', (np.var(lenghtvec), np.std(lenghtvec)))
```

Запрос данных из базы данных `Metrics`:
```
cursor.execute('SELECT * FROM Metrics')
metrics_fromdb = cursor.fetchall()
print(metrics_fromdb)
```

# Примеры тестов и результаты
---
Тест №1

Входное изображение:

![test_image](https://github.com/AlenaKV/TestQA/blob/main/Images/test_picture.jpg)

Выходное изображение с отображением найденной точки центра пятна:

![test_image_result](https://github.com/AlenaKV/TestQA/blob/main/Images/test_image_result.png)

#### Данные дисперсии и стандартного отклонения длины радиуса пятна:

Дисперсия:  0.5199182934481335 

Стандартное отклонение:  0.7210535995667267

---
Тест №2

Входное изображение:

![test_image](https://github.com/AlenaKV/TestQA/blob/main/Images/test_picture2.jpg)

Выходное изображение с отображением найденной точки центра пятна:

![test_image_result](https://github.com/AlenaKV/TestQA/blob/main/Images/test_image_result2.png)

#### Данные дисперсии и стандартного отклонения длины радиуса пятна:

Дисперсия:  14.24133821280005

Стандартное отклонение:  3.7737697614984476

---
Тест №3

Входное изображение:

![test_image](https://github.com/AlenaKV/TestQA/blob/main/Images/test_picture3.jpg)

Выходное изображение с отображением найденной точки центра пятна:

![test_image_result](https://github.com/AlenaKV/TestQA/blob/main/Images/test_image_result3.png)

#### Данные дисперсии и стандартного отклонения длины радиуса пятна:

Дисперсия:  0.5760325591165127

Стандартное отклонение:  0.7589680883387079

---
### Результаты заполнения базы данных `Metrics`:

![Screenshot from DB](https://github.com/AlenaKV/TestQA/blob/main/Images/DB.png)

---
## Создание и чтение yaml файла
```
data2 = {
    "metric": {
        "std": 10,
        "var": 10,
    }
}
with open("test.yaml", "w") as file:
    yaml.dump(data2, file)

with open("test.yaml") as file:
    data1 = yaml.safe_load(file)
```


