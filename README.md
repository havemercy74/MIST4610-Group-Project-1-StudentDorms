# MIST4610-Group-Project-1-StudentDorms

Team Name
59925 Group 3

Team Members
David Kenny @djk42296
Sahasra Manchikanti @sahasra-22
Grant Harris @gaharris13
Joseph Enck @havemercy74
Sofia Drescher

Problem Description
The task at hand is to design and develop a relational database for the overall operations of a university housing system. The primary entity in the model is the Residence Hall entity of the housing system, with each Residence Hall representing an individual dormitory facility that the university owns and maintains across campus. The Residence Hall functions alongside dorm floors, student rooms, resident assistants, maintenance requests, and parking assignments that serve the students living within university housing. We aim to accurately represent these relationships, create sample data, and populate the entities and their corresponding attributes with this information. In addition, we intend to perform functional queries on the data so that they may provide meaningful insight into the housing system and its day-to-day operations.

Data Model
The data model represents the core operations of a university housing management system. It is designed to store information about residence halls, dorm floors, rooms, students, resident assistants (RAs), university housing staff, maintenance requests, inspections, and parking assignments. Each of these entities interacts in ways that reflect how a real university housing system functions on a daily basis.
The Residence Hall entity serves as the foundation of the model, with each hall containing multiple Dorm Floors, and each floor consisting of several Rooms. The Students entity stores personal information about each student living on campus, such as their name, phone number, class year, and assigned RA. Each student is connected to a specific room through the DormAssignments table, allowing the database to track which student resides in which room.
Each Resident Assistant is also linked to both a Dorm Floor and a Student record, representing that RAs are students themselves who oversee a particular floor. The Inspections entity stores information about room inspections conducted by RAs, including the inspection score and the room being inspected.
The model also includes MaintenanceRequests, which connect rooms to UGAStaff members responsible for resolving maintenance issues. This allows the system to record who submitted the request, which room it pertains to, and which staff member completed the work. Finally, the Parking entity tracks parking locations and associates them with students who have been assigned parking spaces.
Overall, the database supports storage of all key data related to housing logistics, such as student assignments, staff information, maintenance activity, and inspection records. However, it does not store financial data such as rent payments or billing, nor does it include academic information like student grades or course enrollment. The model focuses strictly on operational and residential management within university housing.

<img width="1041" height="584" alt="505760720-44f5ebf1-4877-4ef4-88e6-f19bc57d6e14" src="https://github.com/user-attachments/assets/bbfb5ca4-bbf8-4e72-8dc7-a82530abfba9" />
