# -IoT-Based-RFID-Smart-Attendance-System
The IoT-Based RFID Smart Attendance System is designed to automate student attendance using RFID technology.
## PROJECT OVERVIEW:

The IoT-Based RFID Smart Attendance System is designed to automate student attendance using RFID technology.

Each student is provided with an RFID card containing a unique UID. When the student scans the card using the RC522 RFID reader, the NodeMCU ESP8266 identifies the student and sends the attendance information to Google Sheets through Wi-Fi.

The system records both Time In and Time Out and displays personalized messages on a 16×2 LCD.
## OBJECTIVE:
- Identifies students using RFID cards.
- Records student attendance automatically.
- Records Time In when the student enters.
- Records Time Out when the student leaves.
- Stores attendance data in Google Sheets.
- Displays personalized messages on an LCD.
- Reduces manual attendance work.

## COMPONENTS REQUIRED:
1. NodeMCU ESP8266	
2. RC522 RFID Reader	
3. RFID Cards/Tags	
4. 16×2 LCD	
5. I2C LCD Module	
6. Breadboard	
7. Jumper Wires
8. USB Cable

## Working of the System:
1. Waiting for Student

Normally, the LCD displays:

RFID Attendance
Tap Your Card

2. Student Scans RFID Card

When a student places the RFID card near the RC522 reader, the unique UID is read.

For example:

3AA71C31

The NodeMCU compares the UID with the registered students.

It identifies:

Brindha Srinithi

3. Time In

When the student enters the classroom and scans the card, the attendance is sent to Google Sheets.

The system records:

Student Name
RFID UID
Date
Time In

The LCD displays:

Welcome
Brindha Srinithi

After 2 seconds:

Your Attendance
Is Recorded

After another 2 seconds:

Have A Nice Day!
Enjoy Your Class

The LCD then returns to:

RFID Attendance
Tap Your Card
Time Out

When the student leaves the classroom, the RFID card is scanned again.

The system identifies the same student and records the Time Out in Google Sheets.

The LCD displays:

Thank You
Brindha Srinithi

After 2 seconds:

Your Time Out
Is Recorded

After 2 seconds:

Hope You Enjoyed
Your Classes!

Finally:

See You Again!
Have A Nice Day

The system then becomes ready for the next student.

## APPLICATIONS:
The project can be used in:

- Schools
- Colleges
- Universities
- Laboratories
- Training centers
- Offices
- Employee attendance systems
## FUTURE ENHANCEMENTS:
The system can be extended with:

- More student RFID cards
- Attendance percentage calculation
- Late-entry detection
- Automatic absent list
- Admin dashboard
- Email/SMS notification
- Monthly attendance reports
- Student registration interface
- Cloud database
- Buzzer for successful/failed scans
- Web-based attendance dashboard
