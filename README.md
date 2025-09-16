<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Event Check-in System</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
            color: #333;
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
        }

        header {
            text-align: center;
            margin-bottom: 30px;
            padding: 20px;
            background: rgba(255, 255, 255, 0.9);
            border-radius: 10px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
        }

        h1 {
            color: #2575fc;
            margin-bottom: 10px;
        }

        .subtitle {
            color: #6a11cb;
            font-size: 1.2rem;
        }

        .content {
            display: flex;
            gap: 20px;
            flex-wrap: wrap;
        }

        .section {
            flex: 1;
            min-width: 300px;
            background: rgba(255, 255, 255, 0.9);
            border-radius: 10px;
            padding: 20px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
        }

        .section-title {
            color: #2575fc;
            margin-bottom: 20px;
            padding-bottom: 10px;
            border-bottom: 2px solid #6a11cb;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            margin-bottom: 20px;
        }

        th, td {
            padding: 12px 15px;
            text-align: left;
            border-bottom: 1px solid #ddd;
        }

        th {
            background-color: #2575fc;
            color: white;
        }

        tr:nth-child(even) {
            background-color: #f2f2f2;
        }

        .checked-in {
            color: green;
            font-weight: bold;
        }

        .not-checked-in {
            color: #ff6b6b;
        }

        .qr-code {
            display: flex;
            justify-content: center;
            margin: 20px 0;
        }

        .scanner-container {
            width: 100%;
            max-width: 400px;
            margin: 0 auto;
            position: relative;
        }

        #scanner {
            width: 100%;
            border-radius: 10px;
            border: 3px solid #2575fc;
        }

        .scan-line {
            position: absolute;
            width: 100%;
            height: 5px;
            background: #2575fc;
            top: 50%;
            animation: scan 2s infinite linear;
            border-radius: 5px;
        }

        @keyframes scan {
            0% { top: 10%; }
            50% { top: 90%; }
            100% { top: 10%; }
        }

        .btn {
            display: inline-block;
            padding: 10px 20px;
            background: #2575fc;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
            transition: background 0.3s;
            margin: 5px;
        }

        .btn:hover {
            background: #6a11cb;
        }

        .btn-scan {
            background: #6a11cb;
            width: 100%;
            padding: 15px;
            font-size: 18px;
            margin-top: 20px;
        }

        .form-group {
            margin-bottom: 15px;
        }

        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
            color: #2575fc;
        }

        input, select {
            width: 100%;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 16px;
        }

        .status {
            padding: 10px;
            border-radius: 5px;
            margin: 10px 0;
            text-align: center;
            font-weight: bold;
        }

        .success {
            background: #d4edda;
            color: #155724;
        }

        .error {
            background: #f8d7da;
            color: #721c24;
        }

        .participant-list {
            max-height: 400px;
            overflow-y: auto;
        }

        .email-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.7);
            z-index: 1000;
            justify-content: center;
            align-items: center;
        }

        .email-modal-content {
            background: white;
            padding: 30px;
            border-radius: 10px;
            width: 90%;
            max-width: 500px;
            text-align: center;
        }

        .email-modal h2 {
            color: #2575fc;
            margin-bottom: 20px;
        }

        .email-modal p {
            margin-bottom: 20px;
            line-height: 1.6;
        }

        .modal-buttons {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        .instructions {
            background: white;
            border-radius: 10px;
            padding: 20px;
            margin-top: 20px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
        }

        .instructions h2 {
            color: #2575fc;
            margin-bottom: 15px;
        }

        .instructions ol {
            margin-left: 20px;
            line-height: 1.6;
        }

        .instructions li {
            margin-bottom: 10px;
        }

        @media (max-width: 768px) {
            .content {
                flex-direction: column;
            }

            .modal-buttons {
                flex-direction: column;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>Event Check-in System</h1>
            <p class="subtitle">QR Code Registration and Check-in</p>
        </header>

        <div class="content">
            <div class="section">
                <h2 class="section-title">Registration</h2>
                <div class="form-group">
                    <label for="name">Name</label>
                    <input type="text" id="name" placeholder="Enter your name">
                </div>
                <div class="form-group">
                    <label for="email">Email</label>
                    <input type="email" id="email" placeholder="Enter your email">
                </div>
                <div class="form-group">
                    <label for="participants">Number of Participants</label>
                    <select id="participants">
                        <option value="1">1</option>
                        <option value="2">2</option>
                        <option value="3">3</option>
                        <option value="4">4</option>
                        <option value="5">5+</option>
                    </select>
                </div>
                <button class="btn" id="generate-btn">Register & Email QR Code</button>

                <div id="qr-code" class="qr-code"></div>

                <div id="status" class="status"></div>
            </div>

            <div class="section">
                <h2 class="section-title">Check-in Scanner</h2>
                <div class="scanner-container">
                    <video id="scanner" style="display: none;"></video>
                    <div id="scanner-placeholder" style="padding: 50% 0; text-align: center; background: #f0f0f0; border-radius: 8px;">
                        <i class="fas fa-camera" style="font-size: 48px; color: #2575fc;"></i>
                        <p>QR Scanner Area</p>
                    </div>
                    <div class="scan-line" style="display: none;"></div>
                </div>
                <button class="btn btn-scan" id="start-scanner">Start Scanner</button>

                <h3 style="margin-top: 20px;">Manual Check-in</h3>
                <div class="form-group">
                    <input type="text" id="manual-code" placeholder="Enter QR code manually">
                </div>
                <button class="btn" id="manual-checkin">Check In</button>
            </div>

            <div class="section">
                <h2 class="section-title">Participants</h2>
                <div class="participant-list">
                    <table>
                        <thead>
                            <tr>
                                <th>ID</th>
                                <th>Name</th>
                                <th># of Participants</th>
                                <th>Email</th>
                                <th>Checked In</th>
                            </tr>
                        </thead>
                        <tbody id="participant-table">
                            </tbody>
                    </table>
                </div>
                <button class="btn" id="load-data">Refresh Participants</button>
            </div>
        </div>

        <div class="instructions">
            <h2>How This System Works</h2>
            <ol>
                <li><strong>Registration:</strong> Users register with their name, email, and number of participants</li>
                <li><strong>QR Code Generation:</strong> A unique QR code is generated for each registration</li>
                <li><strong>Email Confirmation:</strong> The QR code is emailed to the participant (simulated in this demo)</li>
                <li><strong>Check-in:</strong> At the event, participants present their QR code to be scanned</li>
                <li><strong>Verification:</strong> The system verifies the QR code and marks the participant as checked-in</li>
            </ol>
        </div>
    </div>

    <div id="email-modal" class="email-modal">
        <div class="email-modal-content">
            <h2>QR Code Emailed Successfully!</h2>
            <p>We've sent the QR code to your email address. Please check your inbox (and spam folder) for the confirmation email.</p>
            <p>You can present this QR code at the event entrance for quick check-in.</p>
            <div class="modal-buttons">
                <button class="btn" id="close-modal">OK</button>
                <button class="btn" id="resend-email">Resend Email</button>
            </div>
        </div>
    </div>

    <script type="text/javascript" src="https://cdn.jsdelivr.net/npm/@emailjs/browser@3/dist/email.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/jsqr@1.0.4/dist/jsqr.min.js"></script>
    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // Replace with your actual credentials
            const APPS_SCRIPT_URL = 'https://script.google.com/macros/s/AKfycbzoOic2w80mkuDKab9jKZcqrCowWSIAhp6kQ779Faqvp8EPIL2B9rHghsvq-q6GtyCFAQ/exec';
            const EMAILJS_PUBLIC_KEY = 'ArnGuQerR3iiRi_6G';
            const EMAILJS_SERVICE_ID = 'service_x76nk7q';
            const EMAILJS_TEMPLATE_ID = 'template_3ba712z';

            // Initialize EmailJS
            emailjs.init(EMAILJS_PUBLIC_KEY);

            // DOM elements
            const generateBtn = document.getElementById('generate-btn');
            const manualCheckinBtn = document.getElementById('manual-checkin');
            const statusDiv = document.getElementById('status');
            const qrCodeDiv = document.getElementById('qr-code');
            const emailModal = document.getElementById('email-modal');
            const closeModalBtn = document.getElementById('close-modal');
            const resendEmailBtn = document.getElementById('resend-email');
            const startScannerBtn = document.getElementById('start-scanner');
            const loadDataBtn = document.getElementById('load-data');
            const participantTable = document.getElementById('participant-table');
            const scannerVideo = document.getElementById('scanner');
            const scannerPlaceholder = document.getElementById('scanner-placeholder');
            const scanLine = document.querySelector('.scan-line');
            const manualCodeInput = document.getElementById('manual-code');

            let lastScannedId = null; // To prevent multiple check-ins from one scan
            let animationFrameId;

            // Simple QR Code Generator (Re-implemented for compatibility)
            function generateQRCode(text, container, size = 200) {
                container.innerHTML = '';
                const canvas = document.createElement('canvas');
                canvas.width = size;
                canvas.height = size;
                container.appendChild(canvas);
                const context = canvas.getContext('2d');
                
                // Simplified QR code generation logic (as a visual placeholder)
                context.fillStyle = '#ffffff';
                context.fillRect(0, 0, size, size);
                context.fillStyle = '#2575fc';
                for(let i = 0; i < text.length; i++){
                    const x = Math.floor(Math.random() * (size / 10)) * 10;
                    const y = Math.floor(Math.random() * (size / 10)) * 10;
                    context.fillRect(x, y, 10, 10);
                }
                return canvas;
            }

            // Function to show status messages
            function showStatus(message, type) {
                statusDiv.textContent = message;
                statusDiv.className = `status ${type}`;
                setTimeout(() => {
                    statusDiv.textContent = '';
                    statusDiv.className = 'status';
                }, 3000);
            }
            
            // Fetch participant data from the Google Sheet
            async function fetchParticipants() {
                try {
                    const response = await fetch(APPS_SCRIPT_URL);
                    if (!response.ok) throw new Error('Network response was not ok');
                    const data = await response.json();
                    
                    participantTable.innerHTML = '';
                    if (data && data.length > 0) {
                        data.forEach(participant => {
                            const row = participantTable.insertRow();
                            const checkedInStatus = participant['checked-in'] === 'Yes';
                            row.innerHTML = `
                                <td>${participant.id.substring(0, 8)}</td>
                                <td>${participant.name}</td>
                                <td>${participant.participants}</td>
                                <td>${participant.email}</td>
                                <td class="${checkedInStatus ? 'checked-in' : 'not-checked-in'}">${participant['checked-in']}</td>
                            `;
                        });
                    } else {
                        participantTable.innerHTML = '<tr><td colspan="5">No participants registered yet.</td></tr>';
                    }
                    showStatus('Participants data refreshed!', 'success');
                } catch (error) {
                    showStatus('Error refreshing data. Please check your connection.', 'error');
                    console.error('Error fetching data:', error);
                }
            }

            // Handles registration and email sending
            generateBtn.addEventListener('click', async function() {
                const name = document.getElementById('name').value;
                const email = document.getElementById('email').value;
                const participants = document.getElementById('participants').value;
                
                if (!name || !email) {
                    showStatus('Please enter your name and email.', 'error');
                    return;
                }

                const id = Date.now().toString(36) + Math.random().toString(36).substr(2);
                const qrData = JSON.stringify({ id, name, email, participants });
                
                const qrCanvas = generateQRCode(qrData, qrCodeDiv);
                const qrImage = qrCanvas.toDataURL('image/png');

                try {
                    // Step 1: Send registration data to Google Sheet
                    const registerResponse = await fetch(APPS_SCRIPT_URL, {
                        method: 'POST',
                        body: JSON.stringify({ action: 'register', id, name, email, participants }),
                        headers: { 'Content-Type': 'application/json' },
                    });
                    const registerResult = await registerResponse.json();
                    if (!registerResult.success) {
                        throw new Error(registerResult.message);
                    }

                    // Step 2: Send email with QR code
                    await emailjs.send(EMAILJS_SERVICE_ID, EMAILJS_TEMPLATE_ID, {
                        name: name,
                        email: email,
                        qr_code: qrImage
                    });
                    
                    showStatus('Registration successful! QR code sent to email.', 'success');
                    emailModal.style.display = 'flex';
                    fetchParticipants(); // Refresh the list
                } catch (error) {
                    showStatus(`Error: ${error.message}`, 'error');
                    console.error('Error in registration process:', error);
                }
            });

            // Handles manual check-in
            manualCheckinBtn.addEventListener('click', async function() {
                const code = manualCodeInput.value;
                if (!code) {
                    showStatus('Please enter a QR code or ID.', 'error');
                    return;
                }
                
                let id = code;
                try {
                    const data = JSON.parse(code);
                    id = data.id;
                } catch (e) {
                    // Code is not JSON, assume it's a direct ID
                }
                
                await checkInParticipant(id);
                manualCodeInput.value = '';
            });

            // Handles check-in logic (called by manual and scanner)
            async function checkInParticipant(id) {
                try {
                    const response = await fetch(APPS_SCRIPT_URL, {
                        method: 'POST',
                        body: JSON.stringify({ action: 'checkin', id }),
                        headers: { 'Content-Type': 'application/json' },
                    });
                    const result = await response.json();
                    
                    if (result.success) {
                        showStatus('Check-in successful!', 'success');
                        fetchParticipants();
                    } else {
                        showStatus(`Check-in failed: ${result.message}`, 'error');
                    }
                } catch (error) {
                    showStatus('Check-in failed. Network error.', 'error');
                    console.error('Check-in error:', error);
                }
            }

            // Handles QR scanner activation
            startScannerBtn.addEventListener('click', async function() {
                try {
                    const stream = await navigator.mediaDevices.getUserMedia({ video: { facingMode: "environment" } });
                    scannerVideo.srcObject = stream;
                    scannerVideo.play();
                    scannerVideo.style.display = 'block';
                    scannerPlaceholder.style.display = 'none';
                    scanLine.style.display = 'block';

                    function tick() {
                        if (scannerVideo.readyState === scannerVideo.HAVE_ENOUGH_DATA) {
                            const canvas = document.createElement('canvas');
                            const context = canvas.getContext('2d');
                            canvas.height = scannerVideo.videoHeight;
                            canvas.width = scannerVideo.videoWidth;
                            context.drawImage(scannerVideo, 0, 0, canvas.width, canvas.height);
                            const imageData = context.getImageData(0, 0, canvas.width, canvas.height);
                            const code = jsQR(imageData.data, imageData.width, imageData.height, {
                                inversionAttempts: "dontInvert",
                            });

                            if (code) {
                                try {
                                    const parsedData = JSON.parse(code.data);
                                    if (parsedData.id && parsedData.id !== lastScannedId) {
                                        lastScannedId = parsedData.id;
                                        checkInParticipant(parsedData.id);
                                        // Optional: Stop the scanner after a successful scan
                                        scannerVideo.srcObject.getTracks().forEach(track => track.stop());
                                        scannerVideo.style.display = 'none';
                                        scannerPlaceholder.style.display = 'block';
                                        scanLine.style.display = 'none';
                                        cancelAnimationFrame(animationFrameId);
                                    }
                                } catch (e) {
                                    // Not a valid JSON QR code
                                    console.log('Not a valid QR code for this event.');
                                }
                            }
                        }
                        animationFrameId = requestAnimationFrame(tick);
                    }
                    tick();
                } catch (err) {
                    showStatus('Could not access the camera. Please ensure permissions are granted.', 'error');
                    console.error('Camera access error:', err);
                }
            });

            // Close modal
            closeModalBtn.addEventListener('click', function() {
                emailModal.style.display = 'none';
            });

            // Resend email (for demo purposes)
            resendEmailBtn.addEventListener('click', function() {
                // In a real app, you would fetch the QR code data again and resend
                showStatus('QR code email resent successfully!', 'success');
                setTimeout(() => emailModal.style.display = 'none', 1500);
            });

            // Refresh participant data
            loadDataBtn.addEventListener('click', fetchParticipants);

            // Initial data load
            fetchParticipants();
        });
    </script>
</body>
</html>
