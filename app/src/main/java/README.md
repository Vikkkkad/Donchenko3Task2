# Лабораторная работа №3. Работа с базой данных. Задание 2
## Выполнила: Донченко Вика, ИСП-211о
## Язык программирования: Java

### 1. Функции приложения
Приложение состоит из двух экранов:
- "Меню" (MainActivity).
- "Экран 2" (MainActivity2).

Приложение создает базу данных и таблицу "Одногруппники", если они не существуют. Также удаляет все записи и добавляет 5 новых при первом запуске.
### 2. Меню
Меню состоит из трех кнопок:
- "Посмотреть все записи" - происходит переход между activity и выводятся данные из таблицы

  `btnShowRecords.setOnClickListener(v -> {

  Intent intent = new Intent(MainActivity.this, MainActivity2.class);

  startActivity(intent);

  });`

- "Добавить новую запись" - добавляется новая запись.

  `btnAddRecord.setOnClickListener(v -> {
  ContentValues contentValues = new ContentValues();
  contentValues.put("surname", "Новая Фамилия");
  contentValues.put("name", "Новое Имя");
  contentValues.put("patronymic", "Новое Отчество");
  long newRowId = db.insert("classmates", null, contentValues);
  if (newRowId != -1) {
  Toast.makeText(MainActivity.this, "Студент добавлен", Toast.LENGTH_SHORT).show();
  } else {
  Toast.makeText(MainActivity.this, "Ошибка добавления студента", Toast.LENGTH_SHORT).show();
  }
  });`

- "Обновить последнюю запись" - происходит замена старой записи на новую.

  `private boolean updateLastRecord() {
  Cursor cursor = db.rawQuery("SELECT * FROM classmates ORDER BY ID DESC LIMIT 1", null);
  boolean updated = false;
  if (cursor != null) {
  try {
  if (cursor.moveToFirst()) {
  @SuppressLint("Range") int id = cursor.getInt(cursor.getColumnIndex("ID"));
  ContentValues contentValues = new ContentValues();
  contentValues.put("surname", "Иванов");
  contentValues.put("name", "Иван");
  contentValues.put("patronymic", "Иванович");
  int rowsAffected = db.update("classmates", contentValues, "ID = ?", new String[]{String.valueOf(id)});
  updated = rowsAffected > 0;
  }
  } finally {
  cursor.close();
  }
  }`

![activity1.png](..%2F..%2F..%2F..%2Factivity1.png)

### 3. Второй экран
Создается таблица "Одногруппники" с полями:

- ID;
- Имя;
- Фамилия;
- Отчество;
- Время добавления записи.
` private void loadRecords() {
  Cursor cursor = db.rawQuery("SELECT * FROM classmates", null);
  if (cursor != null) {
  try {
  if (cursor.moveToFirst()) {
  do {
  @SuppressLint("Range") String surname = cursor.getString(cursor.getColumnIndex("surname"));
  @SuppressLint("Range") String name = cursor.getString(cursor.getColumnIndex("name"));
  @SuppressLint("Range") String patronymic = cursor.getString(cursor.getColumnIndex("patronymic"));
  @SuppressLint("Range") String addedTime = cursor.getString(cursor.getColumnIndex("added_time"));
  String fio = surname + " " + name + " " + patronymic + " - " + addedTime;
  studentList.add(fio);
  } while (cursor.moveToNext());
  }
  adapter.notifyDataSetChanged();
  } finally {
  cursor.close();
  }
  }`

![activity2_1.png](..%2F..%2F..%2F..%2Factivity2_1.png)

### 4. Работа кнопок
- "Добавить новую запись"

При нажатии на данную кнопку высвечивается надпись об успешном добавлении студента.

![добав.png](..%2F..%2F..%2F..%2F%E4%EE%E1%E0%E2.png)

Затем в таблицу, расположенную на MainActivity2, добавляется новая строка.

![activity2_2.png](..%2F..%2F..%2F..%2Factivity2_2.png)

- "Обновить последнюю запись"

При нажатии на данную кнопку высвечивается надпись об успешном обновлении записи.

![обнов.png](..%2F..%2F..%2F..%2F%EE%E1%ED%EE%E2.png)

Затем в таблице обновляются данные.

![activity2_3.png](..%2F..%2F..%2F..%2Factivity2_3.png)