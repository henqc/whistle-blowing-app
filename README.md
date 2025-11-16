# Whistle Blowing App

A Django-based web application for environmental whistleblowing that allows users to submit anonymous or authenticated reports about environmental issues. The application supports Google OAuth authentication, file uploads (text, PDF, and JPEG), and provides an admin interface for managing and tracking report statuses.

## Key Features

- **Google OAuth Authentication**: Secure login using Google accounts via django-allauth
- **Report Submission**: Users can submit reports with titles, descriptions, and file attachments (TXT, PDF, JPEG)
- **Report Management**: Admin users can view, update status, and add notes to reports
- **File Storage**: Reports and attachments are stored on AWS S3
- **Status Tracking**: Reports can be tracked through statuses: New, In Progress, and Resolved
- **User Reports**: Authenticated users can view and manage their own submitted reports

## Tech Stack

### Backend

- **Django**: Web framework
- **Python**: Programming language
- **SQLite**: Local development database
- **PostgreSQL**: Production database (via Heroku)

### Authentication & Authorization

- **django-allauth**: Social authentication (Google OAuth)
- **Django Authentication**: Built-in user authentication system

### File Storage

- **AWS S3**: Cloud storage for report files and attachments
- **boto3**: AWS SDK for Python
- **django-storages**: Django storage backends for S3
- **Pillow**: Image processing library

### Frontend

- **django-bootstrap5**: Bootstrap 5 integration for Django templates
- **HTML Templates**: Server-side rendered templates

### Deployment

- **Heroku**: Cloud platform for deployment
- **gunicorn**: Python WSGI HTTP Server
- **django-heroku**: Heroku-specific Django utilities
- **WhiteNoise**: Static file serving middleware

### Database Management

- **dj-database-url**: Database URL parsing for Django

## Repository Structure

```
whistle-blowing-app/
├── manage.py                 # Django management script
├── Procfile                  # Heroku deployment configuration
├── requirements.txt          # Python dependencies
├── README.md                 # Project documentation
│
├── myapp/                    # Main Django project configuration
│   ├── __init__.py
│   ├── settings.py           # Django settings (database, apps, middleware, etc.)
│   ├── urls.py               # Root URL configuration
│   ├── wsgi.py               # WSGI configuration for deployment
│   └── asgi.py               # ASGI configuration
│
└── whistleblowingapp/        # Main application
    ├── __init__.py
    ├── admin.py              # Django admin configuration
    ├── apps.py               # App configuration
    ├── models.py             # Database models (Report, User, ReportForm)
    ├── views.py              # View functions (index, submitreport, viewreport, etc.)
    ├── urls.py               # App URL routing
    ├── tests.py              # Unit tests
    │
    ├── migrations/           # Database migrations
    │   ├── __init__.py
    │   ├── 0001_initial.py
    │   ├── 0002_report.py
    │   ├── 0003_alter_report_reporttext.py
    │   ├── 0003_remove_report_attachment_remove_report_description_and_more.py
    │   ├── 0004_merge_20240316_1637.py
    │   └── 0005_report_admin_notes_report_reportjpeg_and_more.py
    │
    └── templates/            # HTML templates
        └── whistleblowingapp/
            ├── index.html              # Landing page
            ├── signedin.html           # Post-login dashboard
            ├── submitreport.html       # Report submission form
            ├── submitted.html          # Submission confirmation
            ├── allreports.html         # All reports view (admin)
            ├── viewreport.html         # Individual report view
            ├── viewUserReports.html    # User's own reports
            └── deleteReport.html       # Report deletion confirmation
```

## Environment Variables

The application requires the following environment variables (set via `.env` file or Heroku config):

- `SECRET_KEY`: Django secret key for cryptographic signing
- `AWS_ACCESS_KEY_ID`: AWS access key for S3
- `AWS_SECRET_ACCESS_KEY`: AWS secret key for S3
- `AWS_STORAGE_BUCKET_NAME`: S3 bucket name for file storage
- `DATABASE_URL`: Database connection URL (automatically set by Heroku)

## Getting Started

1. Install dependencies:

   ```bash
   pip install -r requirements.txt
   ```

2. Set up environment variables in a `.env` file

3. Run migrations:

   ```bash
   python manage.py migrate
   ```

4. Create a superuser (optional):

   ```bash
   python manage.py createsuperuser
   ```

5. Run the development server:
   ```bash
   python manage.py runserver
   ```

## Deployment

The application is configured for Heroku deployment. The `Procfile` specifies:

- `release`: Runs database migrations before deployment
- `web`: Starts the gunicorn server

## Security Notes

- All secret keys and sensitive credentials are stored in environment variables
- AWS credentials are never hardcoded in the codebase
- The application uses Django's built-in CSRF protection
- Sessions expire when the browser closes
