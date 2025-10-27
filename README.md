# MIST4610-Group-Project-1-StudentDorms

**Team Name**
59925 Group 3

**Team Members**
David Kenny @djk42296
Sahasra Manchikanti @sahasra-22
Grant Harris @gaharris13
Joseph Enck @havemercy74
Sofia Drescher

**Problem Description**
The task at hand is to design and develop a relational database for the overall operations of a university housing system. The primary entity in the model is the Residence Hall entity of the housing system, with each Residence Hall representing an individual dormitory facility that the university owns and maintains across campus. The Residence Hall functions alongside dorm floors, student rooms, resident assistants, maintenance requests, and parking assignments that serve the students living within university housing. We aim to accurately represent these relationships, create sample data, and populate the entities and their corresponding attributes with this information. In addition, we intend to perform functional queries on the data so that they may provide meaningful insight into the housing system and its day-to-day operations.

**Data Model**
The data model represents the core operations of a university housing management system. It is designed to store information about residence halls, dorm floors, rooms, students, resident assistants (RAs), university housing staff, maintenance requests, inspections, and parking assignments. Each of these entities interacts in ways that reflect how a real university housing system functions on a daily basis.
The Residence Hall entity serves as the foundation of the model, with each hall containing multiple Dorm Floors, and each floor consisting of several Rooms. The Students entity stores personal information about each student living on campus, such as their name, phone number, class year, and assigned RA. Each student is connected to a specific room through the DormAssignments table, allowing the database to track which student resides in which room.
Each Resident Assistant is also linked to both a Dorm Floor and a Student record, representing that RAs are students themselves who oversee a particular floor. The Inspections entity stores information about room inspections conducted by RAs, including the inspection score and the room being inspected.
The model also includes MaintenanceRequests, which connect rooms to UGAStaff members responsible for resolving maintenance issues. This allows the system to record who submitted the request, which room it pertains to, and which staff member completed the work. Finally, the Parking entity tracks parking locations and associates them with students who have been assigned parking spaces.
Overall, the database supports storage of all key data related to housing logistics, such as student assignments, staff information, maintenance activity, and inspection records. However, it does not store financial data such as rent payments or billing, nor does it include academic information like student grades or course enrollment. The model focuses strictly on operational and residential management within university housing.

<img width="1041" height="584" alt="505760720-44f5ebf1-4877-4ef4-88e6-f19bc57d6e14" src="https://github.com/user-attachments/assets/bbfb5ca4-bbf8-4e72-8dc7-a82530abfba9" />

**Data Dictionary**

<img width="1318" height="542" alt="image" src="https://github.com/user-attachments/assets/a4acb69f-3101-4a9c-9f86-c2cdf4e480eb" />
<img width="1318" height="774" alt="image" src="https://github.com/user-attachments/assets/becace0f-30b6-4e03-a14f-a01b2a41a87d" />
<img width="1310" height="900" alt="image" src="https://github.com/user-attachments/assets/f911d6ec-4f4e-4350-ba3d-6c0fb966b343" />
<img width="1304" height="736" alt="image" src="https://github.com/user-attachments/assets/5e2bd4d9-5013-43ca-8013-7b7646ce38fd" />
<img width="1310" height="780" alt="image" src="https://github.com/user-attachments/assets/81cf7d19-abb7-4fd3-950b-bf5187831fe9" />
<img width="1314" height="516" alt="image" src="https://github.com/user-attachments/assets/661154e4-92ab-4fe1-a096-80dfe8872920" />
<img width="1296" height="870" alt="image" src="https://github.com/user-attachments/assets/f2d67cef-6069-4eb7-b4f1-02e9ccb16738" />
<img width="1320" height="1078" alt="image" src="https://github.com/user-attachments/assets/21a296b2-bb20-4dc4-9399-e6f36ff60466" />
<img width="1324" height="706" alt="image" src="https://github.com/user-attachments/assets/7f1bc2a8-fc5c-4ed4-a2de-a675d61e8d8a" />

**Queries:**
<img width="1360" height="956" alt="image" src="https://github.com/user-attachments/assets/6dbf5a63-25b2-4da8-986b-5781cea37591" />
QUERY 1 - ResidenceHallRequests
SELECT ResidenceHall.address, COUNT(MaintainanceRequests.requestID) AS NumberOfRequests
FROM MaintainanceRequests
JOIN Rooms ON MaintainanceRequests.roomID = Rooms.roomID
JOIN DormFloor ON Rooms.DormFloor_floorID = DormFloor.floorID
JOIN ResidenceHall ON DormFloor.residenceHallID = ResidenceHall.residenceHallID
GROUP BY ResidenceHall.address
HAVING COUNT(MaintainanceRequests.requestID) > 1;
<img width="1302" height="228" alt="image" src="https://github.com/user-attachments/assets/926c8050-2f76-425a-bca3-e4e3d8dd3eca" />
Query 1 allows managers to see which residence halls have had the most maintenance requests. This helps managers identify which buildings may be experiencing more issues and therefore require more frequent maintenance checks, additional staff, or updated facilities. By listing only halls with more than one request, the query filters out isolated incidents and focuses on areas with recurring problems that may need further investigation.

QUERY 2 - InspectionStats()
SELECT ResidenceHall.address, COUNT(Inspections.inspectionID) AS TotalInspections,
(SELECT COUNT(*) FROM Inspections JOIN Rooms ON Inspections.roomID = Rooms.roomID
JOIN DormFloor ON Rooms.DormFloor_floorID = DormFloor.floorID
WHERE DormFloor.residenceHallID = ResidenceHall.residenceHallID
AND Inspections.inspectionScore = 'Pass') AS TotalPasses
FROM Inspections
JOIN Rooms ON Inspections.roomID = Rooms.roomID
JOIN DormFloor ON Rooms.DormFloor_floorID = DormFloor.floorID
JOIN ResidenceHall ON DormFloor.residenceHallID = ResidenceHall.residenceHallID
GROUP BY ResidenceHall.address, ResidenceHall.residenceHallID
ORDER BY TotalPasses DESC;
<img width="1262" height="284" alt="image" src="https://github.com/user-attachments/assets/b679a792-6995-4828-87d7-0174fea73367" />
Query 2 allows managers to see how many inspections each residence hall has had, along with how many of those inspections passed. This helps managers identify which buildings are performing well during inspections and which may require improvements. Ordering the results by the number of passes allows managers to quickly identify the most compliant residence halls and prioritize those that need additional attention or maintenance.

QUERY 3 - StudentParking
SELECT ResidenceHall.address, COUNT(Students.studentID) AS TotalStudents,
(COUNT(Students.studentID)-(SELECT COUNT(Parking.studentID) FROM Parking
JOIN Students ON Parking.studentID = Students.studentID
JOIN DormFloor ON Students.floorID = DormFloor.floorID
WHERE DormFloor.residenceHallID = ResidenceHall.residenceHallID)) AS StudentsWithoutParking
FROM ResidenceHall
JOIN DormFloor ON ResidenceHall.residenceHallID = DormFloor.residenceHallID
JOIN Students ON Students.floorID = DormFloor.floorID
GROUP BY ResidenceHall.residenceHallID, ResidenceHall.address
ORDER BY StudentsWithoutParking DESC;
<img width="1252" height="346" alt="image" src="https://github.com/user-attachments/assets/74b39a8d-98db-4d22-a9e1-2326b6361759" />

Query 3 allows managers to compare the total number of students in each residence hall to the number of those students who have parking spaces. This provides insight into how many students live in each hall without access to parking, which can help determine if additional parking spaces are needed. If a hall shows a large number of students without parking, it may indicate a need to expand parking options or provide alternative transportation services.

QUERY 4 - RAAssignments
WITH ra_boss AS (SELECT DISTINCT studentID, studentName FROM Students
WHERE studentID IN (SELECT DISTINCT raID FROM Students)), ra_worker AS (SELECT * FROM Students)
SELECT ra_boss.studentName AS "RA", ra_worker.studentName AS "Student"
FROM ra_boss JOIN ra_worker ON ra_worker.raID = ra_boss.studentID
ORDER BY ra_boss.studentName, ra_worker.studentName;
<img width="1128" height="492" alt="image" src="https://github.com/user-attachments/assets/4eae5672-a139-47c8-98bd-8771ddecb160" />
Query 4 allows managers to see which students are Resident Assistants (RAs) and which students are under their supervision. This helps managers visualize the hierarchical structure of RAs and their assigned residents, ensuring that each student is appropriately paired with an RA. Organizing the results by RA name makes it easier to see who manages which students and confirm that all residents are accounted for.

QUERY 5 - StudentMaintenanceRequests
SELECT Students.studentName, Rooms.roomID, MaintainanceRequests.requestID FROM Students
JOIN DormFloor ON Students.floorID = DormFloor.floorID
JOIN Rooms ON DormFloor.floorID = Rooms.DormFloor_floorID
JOIN MaintainanceRequests ON Rooms.roomID = MaintainanceRequests.roomID
ORDER BY Students.studentName, MaintainanceRequests.requestID;
<img width="1170" height="460" alt="image" src="https://github.com/user-attachments/assets/7008b972-5f04-4fee-bc31-7fbc393028d6" />
Query 5 allows managers to identify which students have maintenance requests connected to their rooms. This can be used to track which rooms are experiencing issues and which students are affected. By listing students alongside their room IDs and maintenance request IDs, managers can easily follow up with the responsible staff and ensure that requests are resolved promptly.

QUERY 6 - StaffRequests
SELECT UGAstaff.staffName, ResidenceHall.address, COUNT(MaintainanceRequests.requestID) AS NumRequests FROM MaintainanceRequests
JOIN UGAstaff ON MaintainanceRequests.staffID = UGAstaff.staffID
JOIN Rooms ON MaintainanceRequests.roomID = Rooms.roomID
JOIN DormFloor ON Rooms.DormFloor_floorID = DormFloor.floorID
JOIN ResidenceHall ON DormFloor.residenceHallID = ResidenceHall.residenceHallID
GROUP BY UGAstaff.staffName, ResidenceHall.address
ORDER BY UGAstaff.staffName, ResidenceHall.address;
<img width="1106" height="470" alt="image" src="https://github.com/user-attachments/assets/e99da122-cb40-4c3e-86dd-036ed0db45da" />
Query 6 allows managers to see which staff members have handled the most maintenance requests in each residence hall. This helps identify which staff are most active or which buildings require the most staff attention. Managers can use this information to evaluate workload distribution, recognize high-performing employees, and ensure that maintenance staff are allocated effectively across all halls.

QUERY 7 - GetAllRequests
SELECT MaintainanceRequests.requestID, MaintainanceRequests.roomID, UGAstaff.staffName, UGAstaff.staffPhone
FROM MaintainanceRequests
JOIN UGAstaff ON MaintainanceRequests.staffID = UGAstaff.staffID;
<img width="1320" height="468" alt="image" src="https://github.com/user-attachments/assets/f9b20b44-1f2d-4024-9ae8-d8bbeb25d8fc" />
Query 7 allows managers to view all maintenance requests along with the staff member responsible for each one. This provides an easy way to match staff names and contact information to specific requests, allowing managers to follow up on progress or assign new tasks efficiently.

QUERY 8 - StudentsWithParking
SELECT studentName,
(SELECT lotLocation FROM Parking
WHERE Parking.studentID = Students.studentID) AS lotLocation
FROM Students
WHERE EXISTS (SELECT 1 FROM Parking
WHERE Parking.studentID = Students.studentID);
<img width="1110" height="424" alt="image" src="https://github.com/user-attachments/assets/54276020-f270-4134-9f30-6f98cea10a98" />
Query 8 allows managers to see which students have parking spaces and where those spaces are located. This can help the management team confirm that parking assignments are accurate and up to date. It also ensures that only students who actually have parking spots appear in the results, simplifying record verification.

QUERY 9 - RoomLocations
SELECT Rooms.roomID, DormFloor.floorNumber, ResidenceHall.address
FROM Rooms
JOIN DormFloor ON Rooms.DormFloor_floorID = DormFloor.floorID
JOIN ResidenceHall ON DormFloor.residenceHallID = ResidenceHall.residenceHallID;
<img width="1284" height="474" alt="image" src="https://github.com/user-attachments/assets/6abab128-f58f-4bd8-b43a-6803f5133ba1" />
Query 9 allows managers to view which rooms belong to which floors and which residence halls. This provides a clear understanding of the building layout and structure. It helps management easily identify where each room is located, which is useful for maintenance, inspections, or housing assignments.

QUERY 10 - StudentRooms
SELECT Students.studentName, Rooms.roomID FROM Students
JOIN DormFloor ON Students.floorID = DormFloor.floorID
JOIN Rooms ON DormFloor.floorID = Rooms.DormFloor_floorID;
<img width="1254" height="416" alt="image" src="https://github.com/user-attachments/assets/127bdabe-f96f-47ba-afe0-6aa819bde9de" />
Query 10 allows managers to see which room each student is associated with. This helps confirm that every student is correctly assigned to a room and provides an organized way to verify housing records. It also allows managers to identify which floors and rooms are occupied, assisting with housing capacity planning.
