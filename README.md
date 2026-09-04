# Smart Campus Application

Smart Campus is available in **two versions**:

 1. Single-Organization Version

Designed for a single college, school, or organization.

```text
One Application
      ↓
One Organization
      ↓
Students + Admin
      ↓
Complaints + Service Requests
2. Multi-Organization Version

Designed for a customer who manages multiple organizations from one application.

One Application
      ↓
One Customer Database
      ↓
Multiple Organizations
      ↓
Organization Admins + Students
      ↓
Complaints + Service Requests
📥 Download & Run Application

Multi-Organization Version

👉 Download Smart Campus App (.jar)
https://drive.google.com/file/d/1R2yXtJ-DZXaP24lsQP3a0dAo43VT5Lgi/view?usp=sharing

Single-Organization Version

👉 Download Smart Campus Single Version (.jar)
https://drive.google.com/file/d/1R2yXtJ-DZXaP24lsQP3a0dAo43VT5Lgi/view?usp=sharing

System Requirements
Windows, macOS, or Linux
Java 17 or higher
For building from source: Maven 3.9+
Internet connection is required during the first Maven build to download dependencies.
🚀 How to Run the App
Option 1: Run the .jar
Download the Smart Campus .jar file.
Make sure Java is installed.
Double-click the .jar file.

If double-click does not work, open Command Prompt/Terminal:

java -jar smart-campus.jar

The application automatically creates/uses its SQLite database.

🔐 Multi-Organization Version

The Multi-Organization version uses a role-based architecture.

Super Admin

The Super Admin has overall control of the customer's database.

Manage organizations
Manage organization administrators
Manage users
View organization data
Monitor complaints
Monitor service requests
View audit information
Organization Admin

An Organization Admin can manage only their assigned organization.

View student complaints
Update complaint status
Manage service requests
View students belonging to their organization
Student

Students can:

Login securely
Submit complaints
View their complaints
Track complaint status
Submit service requests
Track service request status
Change their password
🗄️ Database Architecture

Smart Campus uses SQLite for persistent local storage.

Multi-Organization Version

Each customer has their own database.

Customer 1
    └── Database 1
         ├── Organization A
         ├── Organization B
         └── Organization C

Customer 2
    └── Database 2
         ├── Organization X
         └── Organization Y

Customer databases are completely separate.

The same application .jar can therefore be distributed to different customers without sharing their data.

🛡️ Security

The Multi-Organization version includes:

Password hashing
Role-based authorization
Organization-level data isolation
Prepared SQL statements
Foreign-key relationships
Audit logging

Passwords should never be stored as plain text in production.

💾 Data Persistence

Application data is stored in SQLite.

Closing and reopening the application does not remove the data.

The database contains information such as:

Users
Organizations
Complaints
Service requests
Audit logs

Important: Do not upload a customer's real .db file to a public GitHub repository.

🛠️ Built With
Language: Java
GUI: Java Swing / AWT
Database: SQLite
Build Tool: Apache Maven
Password Security: BCrypt
Architecture: DAO + Service + Model
IDE: IntelliJ IDEA / VS Code
📂 Project Structure
Single Version
SINGLE_VERSION/
├── src/
│   └── SmartCampusApp.java
├── pom.xml
├── README.md
└── .gitignore
Multi-Organization Version
MULTI_VERSION/
├── src/
│   └── main/
│       └── java/
│           └── com/
│               └── smartcampus/
│                   ├── Main.java
│                   ├── dao/
│                   ├── db/
│                   ├── model/
│                   ├── security/
│                   ├── service/
│                   └── ui/
├── pom.xml
├── README.md
└── .gitignore
💻 Build from Source
Clone the Repository
git clone https://github.com/YOUR_USERNAME/SmartCampus.git
cd SmartCampus
Multi-Organization Version
cd MULTI_VERSION
mvn clean package

Run:

java -jar target/smart-campus.jar
Single-Organization Version
cd SINGLE_VERSION
mvn clean package

Run:

java -jar target/smart-campus-single.jar
🔑 First-Time Setup
Multi-Organization Version

On the first launch:

The application creates the SQLite database.
The Super Admin setup screen appears.
Create the Super Admin account.
Login using the newly created account.
Create organizations.
Create Organization Admin accounts.
Add/register students.
Students can then use their accounts to access the application.

On subsequent launches, the application directly displays the login screen.

🧪 Testing

The application can be tested using the following workflow:

First Launch
     ↓
Super Admin Setup
     ↓
Super Admin Login
     ↓
Create Organization
     ↓
Create Organization Admin
     ↓
Create Student
     ↓
Student Login
     ↓
Submit Complaint
     ↓
Organization Admin
     ↓
Update Complaint Status
⚠️ Important Notes
SQLite is intended for local/small-scale deployment.
The .db database should not be committed to a public GitHub repository.
Do not commit passwords, API keys, or other secrets.
The Multi-Organization version is recommended when one customer needs to manage multiple organizations.
If multiple computers need to access the same live database simultaneously over a network, a server-based database/backend architecture such as MySQL or PostgreSQL would be more appropriate.
📌 Project Versions
Version	Purpose
Single Version	One organization per application/database
Multi Version	Multiple organizations within one customer's database
👨‍💻 Author

Kishore R

Smart Campus Application

📄 License

This project is developed for educational and project demonstration purposes.


This format is much closer to the README you showed: **short introduction → Features → Download → Requirements → Run → Built With → Source Build**, while still documenting your two versions and the actual architecture. Your older Single Version is the single-organization implementation, while the newer uploaded project is the multi-organization implementation. :contentReference[oaicite:0]{index=0} :contentReference[oaicite:1]{index=1}

If you want, I can also make the **actual `README.md` file with GitHub badges, icons, screenshots section, download buttons, and your GitHub username/repository name**, ready to put directly into the repository.
