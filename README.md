# HamSanj - Peer Evaluation System

A web-based application for collecting student peer evaluations on presentations and group work. Built with React and Firebase.

**About the Name**
Hamsanj (هم‌سنج) is a Persian compound word combining:
- Ham (هم) = "together" or "peer/co-"
- Sanj (سنج) = "to measure" (from the verb sanjidan سنجیدن)

Literally translated: "measuring together" or "peer assessment"
While not a common word in everyday Persian, Hamsanj captures the essence of collaborative evaluation—peers measuring and assessing each other's work together. The name reflects both the tool's purpose and the collaborative spirit of academic peer review.


## About This Template

This is a template repository for HamSanj. To use it:

1. **Do NOT use the Firebase configuration in this template** - it contains placeholder values
2. Create your own Firebase project (free) following the setup instructions below
3. Replace the placeholder Firebase config with your own credentials
4. Deploy to your own website or Firebase Hosting

**Note:** Firebase client-side API keys are safe to include in public code. Your data security comes from Firebase Authentication (your login credentials) and Firestore Security Rules (which you'll configure during setup).

## Features

- **Instructor Dashboard**
  - Create courses and sections
  - Bulk import students via CSV
  - Manually add/edit individual students
  - Create custom evaluation criteria with multiple questions
  - Assign different criteria to different sections
  - Export evaluation results to CSV
  - View aggregate scores and detailed evaluations

- **Student Interface**
  - Evaluate classmates' presentations (all section members)
  - Evaluate group members (same group only)
  - Rate each criterion separately on a 1-10 scale
  - Track submitted evaluations

## Prerequisites

- Google account
- Firebase project (free tier available)
- GitHub account (optional, for hosting)
- Basic familiarity with command line (optional, for Firebase CLI deployment)

## Quick Start Summary

1. Create Firebase project → Enable Auth, Firestore, Hosting
2. Set security rules (provided below)
3. Replace Firebase config in `index.html` with yours
4. Create instructor account in Firebase Authentication
5. Deploy to Firebase Hosting or GitHub Pages
6. Login and start adding courses/students

**Estimated setup time:** 15-20 minutes

## Detailed Setup Instructions

### 1. Create Firebase Project

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click "Add project"
3. Enter project name (e.g., "HamSanj-yourname")
4. Disable Google Analytics (optional)
5. Click "Create project"

### 2. Enable Firebase Services

**Enable Authentication:**
1. In Firebase Console, go to **Build** → **Authentication**
2. Click "Get started"
3. Go to **Sign-in method** tab
4. Enable **Email/Password** provider
5. Click "Save"

**Enable Firestore Database:**
1. Go to **Build** → **Firestore Database**
2. Click "Create database"
3. Choose **Start in production mode**
4. Select a location (choose closest to your users)
5. Click "Enable"

**Enable Hosting (Optional):**
1. Go to **Build** → **Hosting**
2. Click "Get started"
3. Follow the setup wizard (you'll complete this later if deploying to Firebase)

### 3. Configure Firestore Security Rules

1. In Firestore Database, go to **Rules** tab
2. Replace the default rules with:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /instructors/{instructorId} {
      allow read, write: if request.auth != null && request.auth.uid == instructorId;
    }
    match /courses/{courseId} {
      allow read: if request.auth != null;
      allow write: if request.auth != null;
    }
    match /sections/{sectionId} {
      allow read: if true;
      allow write: if request.auth != null;
    }
    match /students/{studentId} {
      allow read: if true;
      allow create, update, delete: if request.auth != null;
    }
    match /criteria/{criteriaId} {
      allow read, write: if request.auth != null;
    }
    match /groups/{groupId} {
      allow read: if true;
      allow write: if request.auth != null;
    }
    match /evaluations/{evalId} {
      allow create: if true;
      allow read: if request.auth != null;
      allow update, delete: if request.auth != null;
    }
  }
}
```

3. Click "Publish"

### 4. Get Firebase Configuration

1. In Firebase Console, go to **Project settings** (gear icon)
2. Scroll down to "Your apps"
3. Click the **Web** icon (`</>`)
4. Register your app with a nickname (e.g., "HamSanj")
5. Copy the `firebaseConfig` object - you'll need these values:
   - apiKey
   - authDomain
   - projectId
   - storageBucket
   - messagingSenderId
   - appId
   - measurementId (optional)

### 5. Update Application Code

1. Open the `index.html` file from this repository
2. Find the `firebaseConfig` section (around line 25):

```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT.firebasestorage.app",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID",
  measurementId: "YOUR_MEASUREMENT_ID"
};
```

3. Replace with your actual Firebase config values from step 4

### 6. Create Instructor Account

1. In Firebase Console, go to **Authentication** → **Users**
2. Click "Add user"
3. Enter your email and password (remember these - you'll use them to login)
4. Click "Add user"

### 7. Deploy Your Application

Choose one of the following deployment methods:

#### Option A: Deploy to Firebase Hosting (Recommended)

**Install Firebase CLI:**
```bash
npm install -g firebase-tools
```

**Login to Firebase:**
```bash
firebase login
```

**Initialize your project:**
```bash
cd your-project-directory
firebase init
```

- Select **Hosting** (use spacebar to select, enter to confirm)
- Choose your Firebase project
- Set public directory: `.` (current directory)
- Configure as single-page app: **No**
- Don't overwrite existing files

**Deploy:**
```bash
firebase deploy
```

Your app will be live at: `https://your-project-id.web.app`

#### Option B: Deploy to GitHub Pages

1. Create a new repository on GitHub
2. Push your `index.html` file to the repository
3. Go to **Settings** → **Pages**
4. Select branch: `main`, folder: `/ (root)`
5. Save and wait for deployment (takes 1-2 minutes)
6. Your app will be at: `https://yourusername.github.io/repo-name`

#### Option C: Host on Your Own Website

1. Upload `index.html` to your web server
2. Access it at: `https://yourdomain.com/path/to/index.html`

## Usage Guide

### For Instructors

**First Time Setup:**
1. Navigate to your deployed app URL
2. Log in with your instructor credentials (from step 6)
3. Create a course (e.g., "ECO230 - Data Analysis")
4. Create sections (e.g., "Section 11", "Section 13")
5. Create evaluation criteria:
   - Go to **Criteria** tab
   - Name your criteria (e.g., "Presentation Rubric")
   - Choose type: Presentation or Group
   - Add questions (e.g., "Clarity of presentation", "Use of examples", "Time management")
   - Click "Save Criteria"
6. Assign criteria to sections:
   - Go to **Sections** tab
   - For each section, select which criteria to use from the dropdowns
   - Separate criteria can be assigned for presentations and group work

**Import Students:**
1. Go to **Students** tab
2. Click "Download Template" to get CSV format
3. Fill in student data in Excel/Google Sheets:
   - Name, Email, Student ID, Section, Order, Group
4. Save as CSV
5. Click "Bulk Import CSV"
6. Upload your file
7. Review preview and click "Import"

**Or Add Students Manually:**
1. Go to **Students** tab
2. Click "Add Student" button
3. Fill in student information
4. Click "Add Student"

**Edit Students:**
1. Find student in the list
2. Click "Edit" button
3. Update information
4. Click "Update Student"

**Export Results:**
1. Go to **Reports** tab
2. Select a section from dropdown
3. View overview of all students' scores
4. Click "Export to CSV"
5. Open in Excel/Google Sheets to analyze

### For Students

1. Go to the app URL provided by your instructor
2. Click "Student" tab on the login page
3. Enter your information:
   - Your name
   - Your email (must match exactly what instructor entered)
   - Your section
4. Click "Continue"
5. You'll see two columns:
   - **Presentation Evaluations** - Rate all classmates in your section
   - **Group Member Evaluations** - Rate only your group members (if assigned to a group)
6. For each person:
   - Answer each question with a score from 1-10
   - Click "Submit" when complete
7. Once submitted, you'll see "✓ Submitted" for that person
8. You can logout and return later to complete more evaluations

## CSV Template Format

Download the template from the app, or create a CSV with this format:

```csv
Name,Email,Student ID,Section,Order,Group
John Doe,john@university.edu,12345,Section 11,1,1
Jane Smith,jane@university.edu,12346,Section 11,2,1
Bob Wilson,bob@university.edu,12347,Section 11,3,2
```

**Fields:**
- **Name** - Student's full name (required)
- **Email** - Student's email address (required, case-insensitive)
- **Student ID** - University ID number (optional)
- **Section** - Section name, must match existing section exactly (required)
- **Order** - Presentation order number (optional, default: 999)
- **Group** - Group ID for group evaluations (optional, can be any identifier like "1", "A", "Team1")

## Data Structure

**Firestore Collections:**
- `courses` - Course information linked to instructor
- `sections` - Course sections with assigned criteria IDs
- `students` - Student records with section and group assignments
- `criteria` - Evaluation rubrics with questions array
- `evaluations` - Submitted peer evaluations with individual scores
- `groups` - Group assignments (optional)

**Evaluation Document Structure:**
```javascript
{
  evaluatorEmail: "student@university.edu",
  evaluatedStudentId: "xyz123",
  sectionId: "section123",
  type: "presentation" | "group",
  criteriaId: "criteria123",
  scores: [8, 9, 7], // Individual question scores (1-10)
  score: 8.0, // Calculated average
  timestamp: "2024-10-06T12:00:00Z"
}
```

## Troubleshooting

**"Failed to load data"**
- Check that Firebase security rules are published correctly
- Verify you're logged in as instructor
- Open browser console (F12) for detailed error messages
- Ensure your Firebase project has Firestore enabled

**"Student not found"**
- Verify student email matches exactly (case-insensitive)
- Check that student is assigned to the correct section
- Ensure student was successfully added to database

**No classmates appear for student**
- Add at least 2 students to the section
- Verify all students are in the same section
- For group evaluations, verify students have the same group ID
- Check that criteria is assigned to the section

**"No criteria template found"**
- Go to **Sections** tab as instructor
- Assign criteria to the section using the dropdowns
- Ensure criteria type matches (presentation vs group)
- Verify criteria was saved successfully

**Import fails or skips students**
- Check that section names in CSV match exactly
- Ensure CSV is properly formatted (no extra commas, quotes)
- Verify all required fields (Name, Email, Section) are filled
- Check for duplicate emails

**Can't login as instructor**
- Verify email and password are correct
- Check that user was created in Firebase Authentication
- Clear browser cache and try again
- Ensure you're clicking "Instructor" tab, not "Student"

## Cost Estimation

**Firebase Free Tier (Spark Plan):**
- 50,000 document reads per day
- 20,000 document writes per day
- 1 GB storage
- 10 GB/month hosting bandwidth
- 360 MB/day data transfer

**Typical Usage (per instructor per semester):**
Per evaluation cycle:
- 100 students submitting 99 evaluations each = ~9900 writes
- 100 students submitting 5 group member evaluations each = ~500 writes
- Total: ~10,400 writes per full evaluation cycle

Over a semester (15 weeks): 
- Presentation evaluations (assuming phased, not all at once): ~10,000 writes
- Group evaluations (2-3 cycles): ~1,500 writes
- Instructor viewing reports weekly: ~15,000 reads
- Students logging in to evaluate: ~25,000 reads
- Total per semester: ~11,500 writes, ~40,000 reads

  **Conclusion:** Most instructors will stay well within free tier limits
                  If all 100 students evaluate on the same day, you'd use ~10,400 writes (still under the 20,000 daily limit)

**If usage grows beyond free tier:**
- Happens when: 3-4 instructors with large classes using the same Firebase project simultaneously
- Upgrade to Blaze Plan (pay-as-you-go)
- Pricing: $0.06 per 100K reads, $0.18 per 100K writes
- Estimated cost for heavy usage: $10-25/month for 200-400 students across multiple instructors
- No surprise charges - you can set budget alerts in Firebase Console

## Security & Privacy

- Student emails and names are stored in Firestore
- Evaluations are anonymous to other students (only instructor sees who submitted what)
- Firebase Security Rules prevent unauthorized access
- SSL/HTTPS encryption for all data transmission
- No data is sold or shared with third parties
- Instructors own their data and can export/delete at any time

## Contributing

This project is open source under the MIT License. Contributions welcome:
- Bug fixes and improvements
- Feature additions
- Documentation improvements
- Translations to other languages
- UI/UX enhancements

To contribute:
1. Fork this repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## License

MIT License - free to use, modify, and distribute

## Support

For issues or questions:
- Check the Troubleshooting section above
- Review Firebase Console error messages
- Open an issue on GitHub
- Contact the developer (see acknowledgments below)

## Live Demo

See HamSanj in action at: https://saharnaz.org/resources/apps-forms/

**Note:** This is the developer's production instance. Create your own Firebase project for actual use with your students.

## Screenshots

<img width="460" height="412" alt="image" src="https://github.com/user-attachments/assets/8bab1357-4312-4855-a94b-fa145cdca5a8" />
<img width="454" height="473" alt="image" src="https://github.com/user-attachments/assets/79aae189-7a4b-420b-946e-f89e61f314dd" />
<img width="1278" height="544" alt="image" src="https://github.com/user-attachments/assets/3c75c549-cc6a-451a-9e56-416828b389e3" />


## Acknowledgments

**Developed by:**  
Dr. Saharnaz Babaei-Balderlou  
Assistant Teaching Professor of Economics  
University of Wisconsin - La Crosse  

**Built with:** Claude AI (claude.ai)

**For other instructors:** This tool is free to use and modify. If you find it helpful, consider:
- Sharing it with colleagues who might benefit
- Contributing improvements via GitHub
- Citing it in publications if you use the data for research
- Providing feedback on features or issues

Built for educators to streamline peer evaluation collection and analysis.
