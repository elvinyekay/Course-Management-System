## 🔹 1. FUNKSIONAL MODULLAR
### 👨‍🏫 Müəllim (Admin) Paneli
#### Giriş (login)

- Qrup yaratmaq, baxmaq, redaktə, silmək, arxivləşdirmək

- Hər qrupa tələbə əlavə etmək, redaktə etmək, silmək

#### Hər ay üçün:

- Açıq sual yaratmaq

- Test (variantlı sual) yaratmaq

- Sualları qrupa təyin etmək

- Hər tələbənin nəticələrini izləmək


### 👨‍🎓 Tələbə Paneli
#### Giriş (login)

#### Öz profili

- Aid olduğu qruplarda aktiv sualları cavablandırmaq (test + açıq suallar)

- Cavabları yalnız bir dəfə göndərmək

- Keçmiş nəticələrini görmək



## 🔹 2. TƏRƏFLƏR (Entities və İstifadəçi Rolları)
### 👤 İstifadəçi Rolları:

- Teacher – admin və müəllim

- Student – tələbə


## 🔹 3. MƏLUMAT BAZASI STRUKTURU (Entity Planı)
### 1. Teacher
| Field     | Type                 |
| --------- | -------------------- |
| id        | Long                 |
| name      | String               |
| email     | String (unique)      |
| password  | String (şifrələnmiş) |
| createdAt | Timestamp            |



### 2. Student
| Field     | Type              |
| --------- | ----------------- |
| id        | Long              |
| name      | String            |
| email     | String (unique)   |
| password  | String            |
| group     | Group (ManyToOne) |
| createdAt | Timestamp         |



### 3.Group
| Field      | Type      |
| ---------- | --------- |
| id         | Long      |
| name       | String    |
| isArchived | Boolean   |
| teacher    | Teacher   |
| createdAt  | Timestamp |

### 4. Question (abstract / supertype)
| Field     | Type              |
| --------- | ----------------- |
| id        | Long              |
| title     | String            |
| type      | ENUM (OPEN, TEST) |
| group     | Group             |
| month     | Integer (1–12)    |
| year      | Integer           |
| createdBy | Teacher           |
| createdAt | Timestamp         |


### 5. OpenQuestion (extends Question)
| Field       | Type |
| ----------- | ---- |
| description | Text |


### 6. TestQuestion (extends Question)
| Field         | Type             |
| ------------- | ---------------- |
| questionText  | Text             |
| optionA       | String           |
| optionB       | String           |
| optionC       | String           |
| optionD       | String           |
| correctOption | String (A/B/C/D) |


### 7. Answer
| Field          | Type                        |
| -------------- | --------------------------- |
| id             | Long                        |
| student        | Student                     |
| question       | Question                    |
| textAnswer     | Text (OPEN question cavabı) |
| selectedOption | String (TEST üçün A/B/C/D)  |
| submittedAt    | Timestamp                   |


### 8. Result
| Field        | Type    |
| ------------ | ------- |
| id           | Long    |
| student      | Student |
| month        | Integer |
| year         | Integer |
| correctCount | Integer |
| totalCount   | Integer |


## 🔹 4. TEXNİKİ STRUKTUR
### 📁 Backend Paket Strukturu (Spring Boot):

<pre lang="markdown">
com.example.coursemanager
├── controller
│   ├── AdminController
│   ├── StudentController
│   └── AuthController
├── service
│   ├── GroupService
│   ├── QuestionService
│   ├── AnswerService
├── model
│   ├── Teacher
│   ├── Student
│   ├── Group
│   ├── Question
│   ├── OpenQuestion
│   ├── TestQuestion
│   ├── Answer
│   └── Result
├── repository
│   ├── TeacherRepository
│   ├── StudentRepository
│   ├── GroupRepository
│   ├── QuestionRepository
│   ├── AnswerRepository
│   └── ResultRepository
├── config
│   ├── SecurityConfig
│   └── WebConfig
└── CourseManagerApplication.java
</pre>


### 🧾 Thymeleaf Template-lər:
<pre lang="markdown">
src/
└── main /
└── resources/
└── templates/
├── auth/
│   └── login.html
├── teacher/  
│   ├── dashboard.html
│   ├── group-list.html
│   ├── group-form.html
│   ├── student-list.html
│   ├── student-form.html
│   ├── question-form.html
│   ├── question-list.html
│   ├── result-overview.html
│   └── view-answers.html
└── student/
├── dashboard.html
├── question-list.html
├── answer-form.html
├── result.html
└── profile.html
</pre>


## 🔐 Giriş və İcazə (Authentication / Authorization)
- Spring Security ilə login/logout

- ROLE_TEACHER və ROLE_STUDENT rollarına uyğun kontroller səviyyəsində filtr

- Form login (e-mail və şifrə)


### 🧪 Test və Deployment
| Addım            | Alət                              |
| ---------------- | --------------------------------- |
| Unit Test        | JUnit, Mockito                    |
| Database         | PostgreSQL                        |
| Development      | IntelliJ + Postman                |
| Deployment       | Railway, Render.com               |
| Frontend (local) | Thymeleaf (server-side rendering) |
