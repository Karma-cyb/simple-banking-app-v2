# Simple Banking App

A user-friendly and responsive Flask-based banking application designed for deployment on PythonAnywhere. This application allows users to create accounts, perform simulated money transfers between accounts, view transaction history, and securely manage their credentials.

## Introduction
This project focuses on enhancing the security of an existing web-based banking application called "Simple Banking App". We will collaboratively assess the application for potential vulnerabilities, implement security improvements based on secure development principles, and thoroughly document our findings and the implemented solutions. This project aims to demonstrate our understanding of web application security, vulnerability assessment techniques, and secure coding practices.

## Objectives 
* Conduct a comprehensive security assessment of the existing Simple Banking App.
* Identify and document security vulnerabilities within the application.
* Implement necessary security improvements to address the identified vulnerabilities.
* Demonstrate secure development principles in the implemented improvements.
* Perform penetration testing to validate the effectiveness of the security measures.
* Create detailed documentation of the assessment process, findings, improvements, and testing results.
* Deploy the improved application to a live web hosting platform.

## Features

- **User Authentication**
  - Secure login with username/password
  - Registration of new users
  - Password recovery mechanism (email-based)

- **Account Management**
  - Display of account balance
  - View recent transaction history (last 10 transactions)

- **Fund Transfer**
  - Transfer money to other registered users
  - Confirmation screen before completing transfers
  - Transaction history updated after transfers

- **User Role Management**
  - Regular user accounts
  - Admin users with account approval capabilities
  - Manager users who can manage admin accounts

- **Location Data Integration**
  - Philippine Standard Geographic Code (PSGC) API integration
  - Hierarchical location data selection (Region, Province, City, Barangay)
  - Form fields pre-populated with location data

- **Admin Features**
  - User account approval workflow
  - Account activation/deactivation
  - Deposit funds to user accounts
  - Create new accounts
  - Edit user information

- **Manager Features**
  - Create and manage admin accounts
  - View admin transaction logs
  - Monitor all system transfers

- **Security**
  - Password hashing with bcrypt for secure storage
  - Secure session management
  - Token-based password reset
  - API rate limiting to prevent abuse
  - CSRF protection for all forms

## Security Assessment Findings: Vulnerabilities identified in the original application 
Based on the security enhancements that have been integrated, it can be inferred that the original application likely exhibited several security weaknesses. These potential vulnerabilities included:

*Insecure Password Storage: Passwords might have been stored in plain text or using weak hashing algorithms, leaving them vulnerable to exposure.

*SQL Injection Vulnerabilities: The application was likely susceptible to SQL injection attacks due to improper sanitization or parameterization of database queries.

*Weak Input Validation: Insufficient validation of user inputs could have led to various attacks, including injection attacks, cross-site scripting (XSS), or data integrity issues.

*Inadequate Authentication and Authorization: Access controls might have been rudimentary, lacking robust role-based permissions or proper administrative oversight for user accounts.

*Missing CSRF Protection: The absence of Cross-Site Request Forgery (CSRF) protection could have exposed users to unauthorized actions.

*Insecure Communication: Data transmission between the client and server might have occurred over unencrypted channels (HTTP), compromising data confidentiality.

*Outdated Dependencies: The application could have relied on outdated third-party libraries and frameworks with known security vulnerabilities.

*Lack of Rate Limiting: Without rate limiting, the application would have been susceptible to brute-force attacks on login or registration endpoints.

*Exposure of Sensitive Files: Configuration files containing sensitive information (like database credentials) might have been inadvertently included in public repositories.

##Security Improvements Implemented: Detailed Description
The SimpleBankApp has undergone significant security hardening, incorporating industry best practices and specific enhancements across various layers:

### Secure Data Storage
*Password Hashing: All user passwords are now hashed using the bcrypt algorithm via Flask-Bcrypt before storage. This prevents passwords from being stored in plain text and significantly increases security against credential theft.
*Environment Variables for Secrets: All sensitive credentials, such as database connection strings and secret keys, are loaded from a .env file. This file is explicitly excluded from version control using .gitignore to prevent accidental exposure of sensitive information.
*SQL Injection Protection: By leveraging SQLAlchemy's Object Relational Mapper (ORM) and its inherent use of parameterized queries, the risk of SQL injection attacks is substantially mitigated.
Input Validation
*Server-Side Validation: All user inputs are rigorously validated on the server side using WTForms validators. This includes checks for required fields, correct email formats, and strong password policies, ensuring data integrity and preventing various injection attacks.
*Client-Side Validation: HTML5 validation is enabled on all forms, providing immediate feedback to users and acting as a first line of defense against malformed input.
*Unique Constraints: The database schema enforces unique constraints on usernames and emails, preventing the creation of duplicate accounts.

### Authentication and Authorization
*Robust Authentication: Flask-Login is used for secure user session management. User passwords are securely verified using bcrypt during the login process.
*Account Approval Workflow: New user accounts must be approved by an administrator before they can gain access to the application, adding an extra layer of security and control.
*Role-Based Access Control (RBAC): The application implements a clear RBAC model with three distinct roles: user, admin, and manager. Each role has specific permissions and access to different dashboards and functionalities.
*Route Protection: Flask decorators, such as @login_required and custom role-checking decorators, are used to ensure that only authenticated and appropriately authorized users can access sensitive routes and features.
### Session Management and CSRF Protection
*Secure Session Handling: User sessions are securely managed, typically involving secure, HTTP-only, and expiring session cookies.
*CSRF Protection: Mechanisms are in place to prevent Cross-Site Request Forgery (CSRF) attacks, ensuring that state-changing requests originate from legitimate users.
### Error Handling and Output Encoding
*Custom Error Pages: User-friendly custom error pages are provided, particularly for scenarios like exceeding rate limits, to improve user experience and prevent information disclosure.
*Output Encoding: While not explicitly detailed, proper output encoding is typically implemented to prevent Cross-Site Scripting (XSS) vulnerabilities by sanitizing user-generated content before rendering it in the browser.
Dependency Management
*Pinned Dependencies: All project dependencies are meticulously listed and pinned to secure versions in requirements.txt, ensuring consistent and secure environments.
*Regular Updates and Vulnerability Scanning: A proactive approach to dependency management is maintained through regular checks for outdated packages (pip list --outdated) and the integration of GitHub Dependabot for continuous vulnerability monitoring. All packages have been updated to their latest secure versions, and unused dependencies have been removed.
### Rate Limiting
*Flask-Limiter Integration: Flask-Limiter is employed to protect against brute-force attacks, denial-of-service (DoS), and general abuse.
*Targeted Limits: Strict rate limits have been applied to critical endpoints such as login, registration, and money transfer functionalities.
*Clear User Feedback: Users who exceed rate limits receive clear and informative custom error messages.
### Secure Communication
*HTTPS Enforcement: In production environments, Flask-Talisman is used to enforce HTTPS, ensuring that all client-server communication is encrypted and protected from eavesdropping.
*Secure HTTP Headers: Flask-Talisman also sets a suite of secure HTTP headers (e.g., HSTS, CSP) to further enhance security.
*Secure Cookies: All cookies are explicitly marked as "Secure" to prevent them from being transmitted over unencrypted channels.

### GitHub Workflow
*Version Control and Collaboration: GitHub is utilized for robust version control, efficient issue tracking, and collaborative code review.
*Pull Request Workflow: All code changes are implemented through a pull request (PR) workflow, requiring code review and approval before merging into the main branch. This promotes code quality and helps identify potential security issues early.
*Sensitive File Exclusion: The .env file and other sensitive configuration files are strictly excluded from the repository using .gitignore, preventing accidental exposure of secrets.

## Penetration Testing Report: Summary of Vulnerabilities Identified, Exploitation Steps, and Recommendations

*Summary of Findings: An overview of the overall security posture and a count of critical, high, medium, and low-risk vulnerabilities.
*Detailed Vulnerability Descriptions: For each vulnerability, a clear description, its potential impact, and its Common Weakness Enumeration (CWE) ID.
*Exploitation Steps: Step-by-step instructions on how to reproduce the vulnerability, demonstrating its exploitability.
*Recommendations: Specific, actionable advice for remediation, including code changes, configuration updates, or procedural adjustments.

## Getting Started

### Prerequisites
- Python 3.7+
- pip (Python package manager)
- MySQL Server 5.7+ or MariaDB 10.2+

### Database Setup

1. Install MySQL Server or MariaDB if you haven't already:
   ```
   # For Ubuntu/Debian
   sudo apt update
   sudo apt install mysql-server
   
   # For macOS with Homebrew
   brew install mysql
   
   # For Windows
   # Download and install from the official website
   ```

2. Create a database user and set privileges:
   ```
   mysql -u root -p
   
   # In MySQL prompt
   CREATE USER 'bankapp'@'localhost' IDENTIFIED BY 'your_password';
   GRANT ALL PRIVILEGES ON *.* TO 'bankapp'@'localhost';
   FLUSH PRIVILEGES;
   EXIT;
   ```

3. Update the `.env` file with your MySQL credentials:
   ```
   DATABASE_URL=mysql+pymysql://bankapp:your_password@localhost/simple_banking
   MYSQL_USER=bankapp
   MYSQL_PASSWORD=your_password
   MYSQL_HOST=localhost
   MYSQL_PORT=3306
   MYSQL_DATABASE=simple_banking
   ```

4. Initialize the database:
   ```
   python init_db.py
   ```

### Installation

1. Clone the repository:
   ```
   git clone https://github.com/lanlanjr/simple-banking-app.git
   cd simple-banking-app
   
   # Set up your own repository
   # First, create a new repository named 'simple-banking-app-v2' on GitHub
   
   # Then configure your local repository
   git remote remove origin
   git remote add origin https://github.com/yourusername/simple-banking-app-v2.git
   git remote set-url origin https://yourusername@github.com/yourusername/simple-banking-app-v2.git
   git branch -M main
   git push -u origin main
   
   # Note: Replace 'yourusername' with your actual GitHub username
   ```

2. Install the required packages:
   ```
   pip install -r requirements.txt
   ```

3. Run the application:
   ```
   python app.py
   ```

4. Access the application at `http://localhost:5000`

## Deploying to PythonAnywhere

1. Create a PythonAnywhere account at [www.pythonanywhere.com](https://www.pythonanywhere.com)

2. Upload your code using Git:
   ```
   git clone https://github.com/yourusername/simple-banking-app-v2.git
   ```

3. Install requirements:
   ```
   cd simple-banking-app-v2
   pip install -r requirements.txt
   ```

4. Set up MySQL database on PythonAnywhere:
   - Go to the Databases tab in your PythonAnywhere dashboard
   - Create a new MySQL database
   - Note the database name, username, and password
   - Update your .env file with these credentials

5. Initialize your database on PythonAnywhere:
   ```
   python init_db.py
   ```

6. Configure a new web app via the PythonAnywhere dashboard:
   - Select "Manual configuration"
   - Choose Python 3.8
   - Set source code directory to `/home/yourusername/simple-banking-app-v2`
   - Set working directory to `/home/yourusername/simple-banking-app-v2`
   - Set WSGI configuration file to point to your Flask app

7. Add environment variables in the PythonAnywhere dashboard for security

## Usage

### Registration
- Navigate to the registration page
- Enter username, email, and password
- Confirm your password
- Submit the form to create your account (pending admin approval)

### Login
- Enter your username and password
- Click "Sign In"

### Account Overview
- View your current balance
- See your recent transaction history

### Transfer Funds
- Navigate to the Transfer page
- Enter recipient's username or account number
- Enter the amount to transfer
- Confirm the transfer details on the confirmation screen
- Complete the transfer

### Password Reset
- Click "Forgot your password?" on the login page
- Enter your registered email address
- Follow the link in the email (simulated in this demo)
- Create a new password

### Admin Features
- Approve new user registrations
- Activate/deactivate user accounts
- Create new user accounts
- Make over-the-counter deposits to user accounts
- Edit user details including location information

### Manager Features
- Create new admin accounts
- Toggle admin status for users
- View all user transactions
- Monitor and audit admin activities

## User Roles

The system supports three types of user roles:

1. **Regular Users** - Can manage their own account, make transfers, and view their transaction history.

2. **Admin Users** - Have all regular user privileges plus:
   - Approve/reject new user registrations
   - Activate/deactivate user accounts
   - Create new user accounts
   - Make deposits to user accounts
   - Edit user information

3. **Manager Users** - Have all admin privileges plus:
   - Create and manage admin accounts
   - View admin transaction logs
   - Monitor all system transfers
   - System-wide oversight capabilities

## Address Management with PSGC API

The application integrates with the Philippine Standard Geographic Code (PSGC) API to provide standardized address selection for user profiles. The address system follows the Philippine geographical hierarchy:

- Region
- Province
- City/Municipality
- Barangay

This integration ensures addresses are standardized and validates location data according to the Philippine geographical structure.

## Technologies Used

- **Backend**: Python, Flask
- **Database**: MySQL (with SQLAlchemy ORM)
- **Frontend**: HTML, CSS, Bootstrap 5
- **Authentication**: Flask-Login, Werkzeug, Flask-Bcrypt
- **Forms**: Flask-WTF, WTForms
- **Security**: Flask-Limiter for API rate limiting, CSRF protection
- **External API**: PSGC API for Philippine geographic data

- ## Technology Stack: Updated List of Technologies Used

*Backend Framework: Flask
*Database: MySQL
*ORM: SQLAlchemy
*Password Hashing: Flask-Bcrypt (utilizes bcrypt)
*Web Forms: WTForms
*Authentication & Session Management: Flask-Login
*Rate Limiting: Flask-Limiter
*HTTPS Enforcement & Security Headers: Flask-Talisman
*Dependency Management: pip (with requirements.txt), GitHub Dependabot
*Version Control: Git / GitHub
*Environment Variables: .env files

## Rate Limiting

The application uses Flask-Limiter to implement API rate limiting, which protects against potential DoS attacks and abusive bot activity. The rate limits are configured as follows:

- **Login**: 10 attempts per minute
- **Registration**: 5 attempts per minute
- **Password Reset**: 5 attempts per hour
- **Money Transfer**: 20 attempts per hour
- **API Endpoints**: 30 requests per minute
- **Admin Dashboard**: 60 requests per hour
- **Admin Account Creation**: 20 accounts per hour
- **Admin Deposits**: 30 deposits per hour
- **Manager Dashboard**: 60 requests per hour
- **Admin Creation**: 10 admin accounts per hour

By default, the rate limiting data is stored in memory. For production use, it's recommended to use Redis as a storage backend for persistence across application restarts. To enable Redis storage:

1. Install Redis server on your system
2. Update the `.env` file with your Redis URL:
   ```
   REDIS_URL=redis://localhost:6379/0
   ```

If Redis is not available, the application will automatically fall back to in-memory storage.

## License

This project is licensed under the MIT License - see the LICENSE file for details.
