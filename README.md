<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Teacher Registration System</title>
    <link rel="stylesheet" href="static/styles.css">
</head>
<body>
    <h1>Teacher Registration System</h1>

    <!-- Registration Section -->
    <section id="registration">
        <h2>Register Teacher</h2>
        <form id="registerForm" onsubmit="event.preventDefault(); registerTeacher();">
            <!-- Form Fields -->
            <select id="identificationType" required>
                <option value="">Select Type of ID</option>
                <option value="SA ID number">SA ID</option>
                <option value="Passport">Passport</option>
            </select>
            <input type="text" id="id" placeholder="ID/Passport Number" required>
            <input type="text" id="name" placeholder="Name" required>
            <input type="text" id="surname" placeholder="Surname" required>
            <input type="email" id="email" placeholder="Email" required>
            <input type="text" id="contactNumber" placeholder="Contact Number" required>
            <input type="text" id="saceNumber" placeholder="SACE Number" required>
            <input type="text" id="persalNumber" placeholder="PERSAL Number" required>
            <select id="gender" required>
                <option value="">Select Gender</option>
                <option value="Male">Male</option>
                <option value="Female">Female</option>
                <option value="Other">Other</option>
            </select>
            <select id="race">
                <option value="">Select Race</option>
                <option value="African">African</option>
                <option value="White">White</option>
                <option value="Indian">Indian</option>
                <option value="Coloured">Coloured</option>
            </select>
            <select id="disability">
                <option value="">Select Disability</option>
                <option value="Yes">Yes</option>
                <option value="No">No</option>
            </select>
            <select id="ageGroup">
                <option value="">Age Group</option>
                <option value="18-35">18-35</option>
                <option value="36-45">36-45</option>
                <option value="46-55">46-55</option>
                <option value="56+">56+</option>
            </select>
            <input type="datetime-local" id="trainingDate" required>
            <select id="district" required>
                <option value="">Select District</option>
                <option value="tshwaneSouth">Tshwane South</option>
                <option value="johannesburgNorth">Johannesburg North</option>
                <option value="ekurhuleniNorth">Ekurhuleni North</option>
                <option value="sedibengEast">Sedibeng East</option>
                <option value="gautengEast">Gauteng East</option>
                <option value="johannesburgEast">Johannesburg East</option>
            </select>
            <input type="text" id="schoolName" placeholder="School Name" required>
            <select id="designation" required>
                <option value="">Select Designation</option>
                <option value="pl1">PL1</option>
                <option value="pl2">PL2</option>
                <option value="hod">HOD</option>
                <option value="deputyPrincipal">Deputy Principal</option>
                <option value="principal">Principal</option>
            </select>
            <input type="text" id="ictFacilitatorTrainer" placeholder="ICT Facilitator Trainer" required>

            <button type="submit">Register Teacher</button>
        </form>
    </section>

    <!-- Enrollment Section -->
    <section id="enrollment">
        <h2>Enroll Teacher</h2>
        <form id="enrollmentForm" onsubmit="event.preventDefault(); enrollTeacher();">
            <input type="text" id="enrollId" placeholder="Teacher ID" required>
            <input type="text" id="courseName" placeholder="Course ID" required>
            <input type="date" id="enrollmentDate" required>
            <button type="submit">Enroll Teacher</button>
        </form>
    </section>

    <!-- Attendance Section -->
    <section id="attendance">
        <h2>Mark Attendance</h2>
        <form id="attendanceForm" onsubmit="event.preventDefault(); markAttendance();">
            <input type="text" id="attendanceTeacherId" placeholder="Teacher ID" required>
            <select id="attendanceStatus" required>
                <option value="">Select Attendance Status</option>
                <option value="Present">Present</option>
                <option value="Absent">Absent</option>
            </select>
            <button type="submit">Mark Attendance</button>
        </form>
    </section>

    <!-- Certificate Section -->
    <section id="certificate">
        <h2>Generate Certificate</h2>
        <form id="certificateForm" onsubmit="event.preventDefault(); generateCertificate();">
            <input type="text" id="certificateTeacherId" placeholder="Teacher ID" required>
            <input type="text" id="certificateCourse" placeholder="Course ID" required>
            <button type="submit">Generate Certificate</button>
        </form>
    </section>

    <!-- JavaScript Code -->
    <script>
        // General fetch function for POST requests
        function postData(url, data) {
            return fetch(url, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(data)
            })
            .then(response => {
                if (!response.ok) throw new Error(`HTTP error! Status: ${response.status}`);
                return response.json();
            });
        }

        // Register Teacher
        function registerTeacher() {
            const teacherData = {
                identificationType: document.getElementById("identificationType").value,
                id: document.getElementById("id").value,
                name: document.getElementById("name").value,
                surname: document.getElementById("surname").value,
                email: document.getElementById("email").value,
                contactNumber: document.getElementById("contactNumber").value,
                saceNumber: document.getElementById("saceNumber").value,
                persalNumber: document.getElementById("persalNumber").value,
                gender: document.getElementById("gender").value,
                race: document.getElementById("race").value,
                disability: document.getElementById("disability").value,
                ageGroup: document.getElementById("ageGroup").value,
                training_date: document.getElementById("trainingDate").value,
                district: document.getElementById("district").value,
                schoolName: document.getElementById("schoolName").value,
                designation: document.getElementById("designation").value,
                ictFacilitatorTrainer: document.getElementById("ictFacilitatorTrainer").value
            };

            postData('/teachers', teacherData)
            .then(data => {
                alert(data.message);
                if (data.message === "Teacher registered successfully") document.getElementById("registerForm").reset();
            })
            .catch(error => alert("Error registering teacher: " + error.message));
        }

        // Enroll Teacher
        function enrollTeacher() {
            const enrollmentData = {
                teacherId: document.getElementById("enrollId").value,
                courseName: document.getElementById("courseName").value,
                enrollmentDate: document.getElementById("enrollmentDate").value
            };

            postData('/enrollment', enrollmentData)
            .then(data => {
                alert(data.message);
                document.getElementById("enrollmentForm").reset();
            })
            .catch(error => alert("Error enrolling teacher: " + error.message));
        }

        // Mark Attendance
        function markAttendance() {
            const attendanceData = {
                teacherId: document.getElementById("attendanceTeacherId").value,
                status: document.getElementById("attendanceStatus").value
            };

            postData('/attendance', attendanceData)
            .then(data => {
                alert(data.message);
                document.getElementById("attendanceForm").reset();
            })
            .catch(error => alert("Error marking attendance: " + error.message));
        }

        // Generate Certificate
        function generateCertificate() {
            const certificateData = {
                teacherId: document.getElementById("certificateTeacherId").value,
                courseId: document.getElementById("certificateCourseId").value
            };

            postData('/certificates', certificateData)
            .then(data => {
                alert(data.message);
                document.getElementById("certificateForm").reset();
            })
            .catch(error => alert("Error generating certificate: " + error.message));
        }
    </script>
</body>
</html>
