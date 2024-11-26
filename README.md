# Hospital_Management_System_Database_SQL_Project

## Project Overview:
This project involves designing and querying a Hospital Management System database. The system manages information related to doctors, patients, medical records, billing, and appointments. The project includes an Entity-Relationship (ER) diagram and a comprehensive set of SQL queries for different levels of complexity.

## ER Diagram:

## Database Structure:
### Tables:
- patients: Contains details like patient_id, name, dob, gender, address, and phone_number.
- doctors: Stores doctor information such as doctor_id, name, specialization, and contact_info.
- appointments: Manages appointment data with fields like appointment_id, doctor_id, patient_id, appointment_date, and status.
- billing: Tracks billing details with bill_id, patient_id, doctor_id, service_date, service_type, amount_charged, and payment_status.
- medical_records: Stores medical data such as record_id, patient_id, doctor_id, diagnosis, treatment_plan, medications_prescribed, and record_date.

## SQL Queries by Level:
### Basic Level:
1. Retrieve all patient details.
```sql
SELECT * FROM patients;
```
2. List all appointments for a specific doctor.
```sql
SELECT * FROM appointments WHERE doctor_id = 8;  -- Replace with desired doctor_id
```
3. Show billing details for a specific patient.
```sql
SELECT * FROM billing WHERE patient_id = 54;  -- Replace with desired patient_id
```
4. Find all patients of a specific gender.
```sql
SELECT * FROM patients WHERE gender = 'Female';
```
5. Display the names and contact information of all doctors.
```sql
SELECT name, contact_info FROM doctors;
```
### Intermediate Level:
  
6. Find all patients who haven’t paid their bills.
```sql
SELECT b.patient_id, p.name, b.payment_status 
FROM billing b 
INNER JOIN patients p ON p.patient_id = b.patient_id 
WHERE payment_status = 'Unpaid';
```
7. Count appointments for each doctor, categorized by status.
```sql
SELECT doctor_id, status, COUNT(status) AS no_of_appointments 
FROM appointments 
GROUP BY doctor_id, status 
ORDER BY doctor_id;
```
8. Retrieve medical records for a specific patient.
```sql
SELECT * FROM medical_records WHERE patient_id = 10667;  -- Replace with desired patient_id
```
9. Calculate total revenue generated by each doctor.
```sql
SELECT d.name, b.doctor_id, SUM(b.amount_charged) AS Total_revenue 
FROM billing b 
INNER JOIN doctors d ON b.doctor_id = d.doctor_id 
GROUP BY d.name, b.doctor_id 
ORDER BY doctor_id;
```
10. Identify patients with more than three appointments.
```sql
SELECT patient_id, COUNT(appointment_id) AS appointment_count 
FROM appointments 
GROUP BY patient_id 
HAVING COUNT(appointment_id) > 3;
```
### Advanced Level:

12. Find patients diagnosed with a specific condition.
```sql
SELECT p.patient_id, p.name, m.diagnosis 
FROM patients p 
INNER JOIN medical_records m ON p.patient_id = m.patient_id 
WHERE diagnosis = 'Diabetes';
```
13. Display appointment details along with billing status.
```sql
SELECT a.*, b.* 
FROM appointments a 
INNER JOIN billing b ON a.patient_id = b.patient_id 
ORDER BY b.patient_id;
```
14. Report patients with pending payments and their last appointment date.
```sql
SELECT p.*, b.payment_status, a.appointment_date 
FROM patients p 
INNER JOIN billing b ON p.patient_id = b.patient_id 
INNER JOIN appointments a ON p.patient_id = a.patient_id 
WHERE b.payment_status = 'Unpaid' 
ORDER BY p.patient_id;
```
15. Find doctors with no appointments in the last 30 days.
```sql
SELECT d.doctor_id, d.name 
FROM doctors d 
INNER JOIN appointments a ON d.doctor_id = a.doctor_id 
WHERE a.appointment_date >= CURRENT_DATE - INTERVAL '30 days';
```
### Scenario-Based Questions:
16. Generate a detailed report showing patient name, doctor specialization, diagnosis, treatment plan, and payment status.
```sql
SELECT p.name, d.specialization, mr.diagnosis, mr.treatment_plan, b.payment_status 
FROM patients p 
INNER JOIN medical_records mr ON p.patient_id = mr.patient_id 
INNER JOIN doctors d ON mr.doctor_id = d.doctor_id 
INNER JOIN billing b ON p.patient_id = b.patient_id AND mr.doctor_id = b.doctor_id;
```
17. Analyze revenue contribution of each service type.
```sql
SELECT b.service_type, SUM(b.amount_charged) AS total_revenue, 
ROUND(CAST(SUM(b.amount_charged) * 100.0 / (SELECT SUM(amount_charged) FROM billing) AS NUMERIC), 2) AS percentage_contribution 
FROM billing b 
GROUP BY b.service_type;
```
## Key Insights and Analysis:
Doctor Performance: Analyze the number of appointments and revenue per doctor.
Patient Data: Track patient visit history and their medical records.
Billing Trends: Understand the revenue generation by different services or doctors.
Appointment Management: Identify appointment trends and status (e.g., confirmed, cancelled).

## Conclusion:
This project provides a scalable and well-structured database solution for hospital management, enabling efficient data handling and insightful analysis of hospital operations.


