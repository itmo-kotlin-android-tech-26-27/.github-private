# Do/Don't

## **Нейминг**

1. Следуйте [правилам именования в Kotlin](https://kotlinlang.org/docs/coding-conventions.html#naming-rules):
   - **Классы**: `PascalCase` (Student, StudentManager)
   - **Функции и переменные**: `camelCase` (studentName, getStudent)
   - **Константы**: `UPPER_SNAKE_CASE` (MAX_STUDENTS)
   - **Пакеты**: `lowercase` (com.example.app)

2. **Не используйте** сокращения и транслит.
   ```kotlin
   // Плохо
   class Thread { fun joinStudent(s: Student) {} }

   // Хорошо
   class StudyGroup { fun addStudent(student: Student) {} }
   ```

3. Используйте **английские термины**: `student.name`, `calculateTotal()`, `userList`

## **Do (Делай так):**

1. **Следуйте кодстайлу** [Kotlin coding conventions](https://kotlinlang.org/docs/coding-conventions.html)
   (для Android — [Android Kotlin style](https://developer.android.com/kotlin/style-guide)).
2. **Семантические коммиты**: `feat: add student validation`, `fix: correct average calculation`
3. **Инициализация через конструктор**:
   ```kotlin
   // Плохо
   val student = Student()
   student.name = "Иван"

   // Хорошо
   class Student(val name: String)
   val student = Student("Иван")
   ```
4. **Свойства приватны по умолчанию** — минимизируйте `public`/`internal`, не открывайте то, что не нужно.
5. **Проверяйте данные** через `require`/`check`:
   ```kotlin
   class Student(val name: String) {
       init {
           require(name.isNotBlank()) { "Name cannot be empty" }
       }
   }
   ```
6. **Guard clauses** вместо вложенности:
   ```kotlin
   // Плохо - вложенность
   if (students != null) {
       if (students.isNotEmpty()) {
           if (students.first() != null) { ... }
       }
   }

   // Хорошо - guard clauses
   if (students.isNullOrEmpty()) return
   val first = students.first()
   ```
7. **Иммутабельность** через `val` и `data class`:
   ```kotlin
   data class Student(val name: String)
   ```
8. **Функции работы с коллекциями вместо циклов**:
   ```kotlin
   // Плохо
   val odds = mutableListOf<Int>()
   for (num in numbers) {
       if (num % 2 != 0) odds.add(num)
   }

   // Хорошо
   val odds = numbers.filter { it % 2 != 0 }
   ```
9. **Null-безопасность вместо `null` вручную**: используйте nullable-типы с безопасными вызовами, а не
   насильное разыменование:
   ```kotlin
   // Плохо
   val name = student!!.name

   // Хорошо
   val name = student?.name ?: "Unknown"
   ```
10. Соблюдайте принципы [DRY, KISS, YAGNI](https://habr.com/ru/articles/144611/):

- **DRY — Don't repeat yourself**. Вместо копирования одного куска кода выносите его в метод для
  дальнейшего переиспользования.
- **KISS — Keep it stupid simple**. Старайтесь делать функции максимально атомарными и не переусложнять
  логику. Большую сложную операцию декомпозируйте на несколько простых.
- **YAGNI — You ain't gonna need this**. Не добавляйте преждевременные абстракции, чтобы поддержать сценарий
  из будущего, для которого ещё неизвестны требования. Проектируйте непосредственно ту предметную область,
  которая описана в текущей задаче.

## **Don't (Не делай так):**

1. **Не используйте магические числа**:
   ```kotlin
   // Плохо
   if (age > 18) { ... }

   // Хорошо
   private const val ADULT_AGE = 18
   if (age > ADULT_AGE) { ... }
   ```

2. **Не ловите и не игнорируйте исключения**:
   ```kotlin
   // Плохо
   try { ... } catch (e: Exception) { } // 🤮

   // Хорошо
   try { ... } catch (e: IllegalArgumentException) {
       logger.error("Invalid input", e)
   }
   ```

3. **Не используйте `null` там, где тип не nullable, и не используйте `!!`**:
   ```kotlin
   // Плохо
   val name: String = student!!.name

   // Хорошо
   val name = student?.name ?: "Unknown"
   ```

4. **Не используйте циклы для поиска**:
   ```kotlin
   // Плохо
   var found: Student? = null
   for (s in students) {
       if (s.id == id) { found = s; break }
   }

   // Хорошо
   return students.firstOrNull { it.id == id }
   ```

5. **Не комментируйте очевидное**:
   ```kotlin
   // Плохо
   fun greet() {
       // Выводит приветствие
       println("Hello!")
   }
   ```

6. **Не используйте примитивы для доменных типов**:
   ```kotlin
   // Плохо - Int для денег
   var price = 100

   // Хорошо - data class
   data class Money(val amount: Int, val currency: String)
   ```
