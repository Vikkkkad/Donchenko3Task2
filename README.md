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

   ![activity1](https://github.com/user-attachments/assets/f99f4af1-669a-441d-b312-373c69f87858)

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

  ![activity2_1](https://github.com/user-attachments/assets/28dfd934-91dc-44e5-a9d8-50ec5882f7cd)

### 4. Работа кнопок
- "Добавить новую запись"

При нажатии на данную кнопку высвечивается надпись об успешном добавлении студента.

   ![добав](https://github.com/user-attachments/assets/a056efb0-801e-46c1-9222-017f9989eea3)

Затем в таблицу, расположенную на MainActivity2, добавляется новая строка.

   ![activity2_2](https://github.com/user-attachments/assets/392be3e8-d971-4c25-8206-0c72fc92043b)

- "Обновить последнюю запись"

При нажатии на данную кнопку высвечивается надпись об успешном обновлении записи.

   ![обнов](https://github.com/user-attachments/assets/4ed974a7-bf4e-4ca0-bcf0-29d133f9b4c9)

Затем в таблице обновляются данные.

   ![activity2_3](https://github.com/user-attachments/assets/7a3d103e-42b0-42fc-95d8-4ef7d863d40c)
