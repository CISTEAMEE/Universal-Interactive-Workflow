// Firebase configuration
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_AUTH_DOMAIN",
    databaseURL: "YOUR_DATABASE_URL",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_STORAGE_BUCKET",
    messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
    appId: "YOUR_APP_ID"
};

// Initialize Firebase
firebase.initializeApp(firebaseConfig);
const database = firebase.database();
const storage = firebase.storage();

// Global variables
let username = '';
let startTime = 0;
let quizStartTime = 0;
let questionStartTime = 0;
let questionEndTime = 0;
let totalQuizTime = 15 * 60; // 15 minutes in seconds
let questionTime = 90; // 90 seconds per question
let currentQuestionIndex = 0;
let quizTimer;
let questionTimer;
let typingTimer;
let typingStartTime = 0;
let typingEndTime = 0;
let typingCharCount = 0;
let typingWordCount = 0;
let wpm = 0;
let diamondsCollected = 0;
let landscapeProgress = 0;
let currentScore = 0;
let isAdmin = false;
let adminPassword = "ecw-admin"; // In a real app, this would be stored securely
let userResults = {
    username: '',
    startTime: '',
    endTime: '',
    totalTime: 0,
    questions: [],
    scores: {
        correctness: 0,
        clarity: 0,
        efficiency: 0,
        total: 0
    },
    typingMetrics: {
        averageWPM: 0,
        totalCharacters: 0,
        totalWords: 0
    },
    rewards: []
};

// Sample questions (in a real app, these would be loaded from an external source)
const questions = [
    {
        id: 1,
        text: "Describe the process to create a new patient in eClinicalWorks. Include all required fields and any best practices.",
        correctAnswer: "1. Log into eClinicalWorks with your credentials.\n2. Click on the 'New Patient' button in the top menu.\n3. Fill in the required fields marked with an asterisk (*):\n   - First Name\n   - Last Name\n   - Date of Birth\n   - Gender\n   - Address\n   - Phone Number\n   - Insurance Information\n4. Verify the patient's insurance by clicking the 'Verify' button.\n5. Add any allergies or medical conditions in the appropriate tabs.\n6. Click 'Save' to create the patient record.\n7. Confirm the patient information is correct on the summary screen."
    },
    {
        id: 2,
        text: "Explain how to schedule a follow-up appointment for a patient in eClinicalWorks. Include steps for finding available slots and setting appointment types.",
        correctAnswer: "1. Open the patient's chart in eClinicalWorks.\n2. Click on the 'Appointments' tab or the calendar icon.\n3. Select 'New Appointment' from the menu.\n4. Choose the appropriate provider from the dropdown menu.\n5. Select the appointment type (e.g., Follow-up, Lab Review).\n6. Choose a date using the calendar interface.\n7. Available time slots will appear - select the preferred time.\n8. Add any notes or specific instructions in the 'Notes' field.\n9. Set the appointment duration if different from the default.\n10. Click 'Save' to confirm the appointment.\n11. Verify the appointment details on the confirmation screen.\n12. Provide the patient with an appointment card or send an electronic reminder."
    },
    {
        id: 3,
        text: "How do you document a patient's vital signs in eClinicalWorks? List all the vital signs that can be recorded and where they appear in the patient's chart.",
        correctAnswer: "1. Open the patient's encounter in eClinicalWorks.\n2. Navigate to the 'Vitals' section (usually found in the left sidebar).\n3. Click 'Add Vitals' or select the current date to update existing vitals.\n4. Record the following vital signs:\n   - Blood Pressure (systolic/diastolic)\n   - Heart Rate (pulse)\n   - Respiratory Rate\n   - Temperature\n   - Oxygen Saturation (SpO2)\n   - Height\n   - Weight\n   - BMI (automatically calculated)\n   - Pain Level (0-10 scale)\n5. Click 'Save' to record the vitals.\n\nThese vitals will appear in:\n1. The Progress Notes section\n2. The Vitals flowsheet for tracking over time\n3. The Visit Summary report\n4. The Patient Dashboard\n5. Any referral letters or continuity of care documents"
    },
    {
        id: 4,
        text: "Describe the process of e-prescribing a medication in eClinicalWorks. Include how to check for drug interactions and set up refills.",
        correctAnswer: "1. Open the patient's encounter in eClinicalWorks.\n2. Click on the 'Prescriptions' or 'Rx' tab in the encounter.\n3. Search for the medication by name in the search field.\n4. Select the appropriate medication from the dropdown results.\n5. Specify the following details:\n   - Dosage\n   - Route of administration\n   - Frequency\n   - Duration\n   - Quantity\n   - Number of refills allowed\n6. Check for drug interactions by clicking the 'Drug Interactions' button.\n7. Review any alerts for allergies, interactions, or duplications.\n8. Add specific instructions for the patient if needed.\n9. Select the pharmacy from the patient's preferred pharmacies list.\n10. Click 'Send' to electronically transmit the prescription.\n11. Document the prescription in the patient's chart.\n12. For refills, set the appropriate number in the 'Refills' field.\n13. For controlled substances, complete the additional authentication steps as required."
    },
    {
        id: 5,
        text: "How do you generate and send a referral in eClinicalWorks? Include the necessary information that should be included in a referral.",
        correctAnswer: "1. Open the patient's encounter in eClinicalWorks.\n2. Navigate to the 'Referrals' section or tab.\n3. Click 'New Referral' to create a referral.\n4. Select the specialty for the referral from the dropdown menu.\n5. Search for and select the specific provider or facility.\n6. Enter the following required information:\n   - Reason for referral (be specific about the clinical question)\n   - Urgency level (routine, urgent, emergent)\n   - Relevant diagnosis codes (ICD-10)\n   - Pertinent clinical information\n7. Attach any relevant documents:\n   - Recent lab results\n   - Imaging reports\n   - Progress notes\n   - Medication list\n8. Specify if authorization is required from insurance.\n9. If needed, complete any insurance authorization forms.\n10. Select the delivery method (fax, electronic, print).\n11. Click 'Send' to transmit the referral.\n12. Document the referral in the patient's chart.\n13. Set up a follow-up task to ensure the referral was received."
    },
    {
        id: 6,
        text: "Explain the process of documenting a telephone encounter in eClinicalWorks. What information should be included and how is it saved to the patient's chart?",
        correctAnswer: "1. Log into eClinicalWorks and search for the patient.\n2. Open the patient's chart.\n3. Click on the 'Telephone Encounters' option or icon.\n4. Select 'New Telephone Encounter' to create a new record.\n5. Document the following essential information:\n   - Date and time of the call\n   - Caller's identity (patient, family member, etc.)\n   - Callback number\n   - Reason for the call\n   - Details of the conversation\n   - Any advice or instructions given\n   - Any prescriptions called in\n   - Follow-up plans\n6. Select the appropriate category for the call (clinical question, prescription refill, test results, etc.).\n7. Assign the telephone encounter to the appropriate provider if needed.\n8. Mark the urgency level (routine, urgent, emergent).\n9. Click 'Save' to document the telephone encounter.\n10. The telephone encounter will be saved in the patient's chart under:\n    - Telephone Encounters section\n    - Progress Notes (if configured)\n    - Patient Timeline\n11. If follow-up is needed, create a task or reminder.\n12. Document any orders or prescriptions that resulted from the call.\n13. Close the encounter when all actions are complete."
    },
    {
        id: 7,
        text: "How do you run a report in eClinicalWorks to identify patients due for preventive screenings? Include specific steps and available report types.",
        correctAnswer: "1. Log into eClinicalWorks with administrative privileges.\n2. Navigate to the Reports module or section.\n3. Select 'Patient Care' or 'Quality of Care' reports category.\n4. Choose 'Preventive Care Measures' or 'Health Maintenance' report.\n5. Set the following parameters:\n   - Date range for the report\n   - Provider(s) to include\n   - Age range of patients\n   - Specific preventive measures (e.g., mammograms, colonoscopies, immunizations)\n   - Insurance types (if filtering by payer)\n6. Select output format (PDF, Excel, etc.).\n7. Click 'Generate Report' to run the report.\n8. Review the report for patients due for screenings.\n9. Available report types include:\n   - Health Maintenance Due report\n   - Quality Measure reports (HEDIS, MIPS, etc.)\n   - Patient Registry reports\n   - Care Planning reports\n   - Population Health reports\n10. Use the 'Recall' feature to generate outreach lists.\n11. Create telephone or letter campaigns for identified patients.\n12. Schedule follow-up reports to track completion rates."
    },
    {
        id: 8,
        text: "Describe how to set up and use Order Sets in eClinicalWorks. What are the benefits of using Order Sets?",
        correctAnswer: "1. Log into eClinicalWorks with administrative privileges.\n2. Navigate to the Admin or Settings module.\n3. Select 'Order Sets' or 'Treatment Plans' configuration.\n4. Click 'New Order Set' to create a new template.\n5. Enter a name and description for the Order Set.\n6. Select the specialty or condition the Order Set applies to.\n7. Add the following components as needed:\n   - Medications (with default dosing)\n   - Lab tests\n   - Diagnostic imaging\n   - Referrals\n   - Patient education materials\n   - Follow-up appointments\n8. Save the Order Set template.\n\nTo use an Order Set:\n1. Open a patient encounter.\n2. Navigate to the Treatment or Plan section.\n3. Click 'Order Sets' or 'Treatment Plans'.\n4. Select the appropriate Order Set from the list.\n5. Modify any components as needed for the specific patient.\n6. Click 'Apply' to add all items to the patient's plan.\n\nBenefits of using Order Sets:\n1. Standardized care based on best practices\n2. Improved efficiency and workflow\n3. Reduced potential for errors or omissions\n4. Consistent documentation across providers\n5. Easier compliance with clinical guidelines\n6. Time savings during patient encounters\n7. Simplified onboarding for new providers\n8. Support for value-based care initiatives"
    },
    {
        id: 9,
        text: "How do you configure and use the Patient Portal in eClinicalWorks? List the features available to patients and how to troubleshoot common issues.",
        correctAnswer: "1. Configure the Patient Portal:\n   - Log in with administrative privileges\n   - Navigate to Portal Administration\n   - Set up portal URL and branding\n   - Configure available features\n   - Set up security protocols\n   - Create patient instruction materials\n\n2. Enroll a patient in the Portal:\n   - Verify patient email in demographics\n   - Click 'Enable Portal Access'\n   - Select enrollment method (in-office or email)\n   - For in-office: print temporary credentials\n   - For email: send invitation with instructions\n   - Document patient consent for portal communication\n\n3. Features available to patients:\n   - Secure messaging with providers\n   - Appointment scheduling and management\n   - Prescription refill requests\n   - Access to lab and test results\n   - View and download visit summaries\n   - Complete pre-visit questionnaires\n   - Update demographic information\n   - View and pay bills online\n   - Access educational materials\n   - Proxy access for family members\n\n4. Common troubleshooting issues:\n   - Login problems: Reset password, verify username\n   - Missing information: Check release settings in EMR\n   - Message not received: Verify correct provider routing\n   - Appointment booking errors: Check scheduling templates\n   - Results not visible: Check release timing settings\n   - Portal access locked: Reset after too many failed attempts\n   - Browser compatibility: Recommend supported browsers\n   - Mobile app issues: Verify version compatibility"
    },
    {
        id: 10,
        text: "Explain how to use the eClinicalWorks Messenger feature for internal communication. Include how to set up groups and manage urgent messages.",
        correctAnswer: "1. Access eClinicalWorks Messenger:\n   - Click the envelope icon in the top menu\n   - Or navigate to the Communication or Messenger module\n\n2. Send a basic message:\n   - Click 'New Message'\n   - Search for and select recipient(s)\n   - Enter subject line\n   - Type message content\n   - Attach files if needed (patient documents, images, etc.)\n   - Set priority (normal, urgent, FYI)\n   - Click 'Send'\n\n3. Create and manage groups:\n   - Navigate to Messenger settings or admin\n   - Select 'Groups' or 'Distribution Lists'\n   - Click 'New Group'\n   - Name the group (e.g., 'Nursing Staff', 'Providers')\n   - Add members by searching and selecting users\n   - Assign a group administrator if needed\n   - Save the group configuration\n   - Groups can be used as recipients for messages\n\n4. Managing urgent messages:\n   - Mark message as 'Urgent' when composing\n   - Configure notification settings for urgent messages\n   - Set up text or email alerts for urgent messages\n   - Create escalation rules for unanswered urgent messages\n   - Establish coverage schedules for urgent message monitoring\n   - Use read receipts to confirm message delivery\n   - Set up auto-forwarding for urgent messages during off hours\n\n5. Best practices:\n   - Keep PHI secure by using internal messaging instead of email\n   - Use standardized subject lines for common communications\n   - Set up message templates for routine communications\n   - Establish clear policies for response times\n   - Train staff on appropriate use of priority levels\n   - Regularly audit message response times\n   - Use 'Out of Office' settings when unavailable"
    }
];

// Reward thresholds and definitions
const rewardThresholds = [
    { score: 20, name: "Purple Diamond Ring", image: "purple-ring.png", class: "purple-ring" },
    { score: 40, name: "Blue-Purple Ring", image: "blue-purple-ring.png", class: "blue-purple-ring" },
    { score: 60, name: "Green Diamond Ring", image: "green-ring.png", class: "green-ring" },
    { score: 80, name: "Big Diamond Stone", image: "big-diamond.png", class: "big-diamond" },
    { score: 100, name: "Diamond Crown", image: "crown.png", class: "crown" }
];

// Landscape phase elements
const landscapePhases = [
    { // Phase 1 (20%)
        elements: [
            { selector: '.sky', action: 'show' },
            { selector: '.mountains', action: 'show' },
            { selector: '.ground', action: 'show' }
        ]
    },
    { // Phase 2 (40%)
        elements: [
            { selector: '.clouds', action: 'show' },
            { selector: '.sun', action: 'show' },
            { selector: '.meadow', action: 'show' }
        ]
    },
    { // Phase 3 (60%)
        elements: [
            { selector: '.snow-cap', action: 'show' },
            { selector: '.river', action: 'show' },
            { selector: '.pine-trees', action: 'show' },
            { selector: '.olive-trees', action: 'show' }
        ]
    },
    { // Phase 4 (80%)
        elements: [
            { selector: '.bridge', action: 'show' },
            { selector: '.palm-trees', action: 'show' },
            { selector: '.roses', action: 'show' },
            { selector: '.cart', action: 'show' }
        ]
    },
    { // Phase 5 (100%)
        elements: [
            { selector: '.palace-foundation', action: 'show' },
            { selector: '.palace-walls', action: 'show' },
            { selector: '.palace-roof', action: 'show' },
            { selector: '.palace-details', action: 'show' },
            { selector: '.flag', action: 'show' },
            { selector: '.user-name-display', action: 'show' },
            { selector: '.cls-medal', action: 'show' }
        ]
    }
];

// DOM Elements
const loginScreen = document.getElementById('login-screen');
const adminLoginScreen = document.getElementById('admin-login-screen');
const adminDashboard = document.getElementById('admin-dashboard');
const instructionsScreen = document.getElementById('instructions-screen');
const quizScreen = document.getElementById('quiz-screen');
const landscapeScreen = document.getElementById('landscape-screen');
const resultsScreen = document.getElementById('results-screen');

const usernameInput = document.getElementById('username');
const adminPasswordInput = document.getElementById('admin-password');
const loginBtn = document.getElementById('login-btn');
const adminLoginLink = document.getElementById('admin-login-link');
const adminLoginBtn = document.getElementById('admin-login-btn');
const backToUserLoginBtn = document.getElementById('back-to-user-login');
const adminLogoutBtn = document.getElementById('admin-logout-btn');
const beginBtn = document.getElementById('begin-btn');
const drawCardBtn = document.getElementById('draw-card-btn');
const answerInput = document.getElementById('answer-input');
const submitAnswerBtn = document.getElementById('submit-answer-btn');
const continueBtn = document.getElementById('continue-btn');
const downloadResultsBtn = document.getElementById('download-results-btn');
const restartQuizBtn = document.getElementById('restart-quiz-btn');
const uploadResourceBtn = document.getElementById('upload-resource-btn');
const resourceFileInput = document.getElementById('resource-file');
const refreshScoresBtn = document.getElementById('refresh-scores-btn');
const downloadAllScoresBtn = document.getElementById('download-all-scores-btn');

const userDisplay = document.getElementById('user-display');
const timerDisplay = document.getElementById('timer-display');
const questionCounterDisplay = document.getElementById('question-counter-display');
const questionText = document.getElementById('question-text');
const questionTimerDisplay = document.getElementById('question-timer');
const wpmDisplay = document.getElementById('wpm-display');
const diamondsCountDisplay = document.getElementById('diamonds-count');
const diamondsDisplay = document.getElementById('diamonds-display');
const rewardsDisplay = document.getElementById('rewards-display');
const finalRewardsDisplay = document.getElementById('final-rewards-display');
const resourcesContainer = document.getElementById('resources-container');
const scoresContainer = document.getElementById('scores-container');

const card = document.querySelector('.card');

// Initialize the application
document.addEventListener('DOMContentLoaded', function() {
    initializeApp();
});

// Prevent pasting in the answer input
answerInput.addEventListener('paste', function(e) {
    e.preventDefault();
    alert('Pasting is not allowed. Please type your answer.');
    return false;
});

// Initialize the application
function initializeApp() {
    // Event listeners
    loginBtn.addEventListener('click', handleLogin);
    adminLoginLink.addEventListener('click', showAdminLogin);
    adminLoginBtn.addEventListener('click', handleAdminLogin);
    backToUserLoginBtn.addEventListener('click', showUserLogin);
    adminLogoutBtn.addEventListener('click', handleAdminLogout);
    beginBtn.addEventListener('click', startQuiz);
    drawCardBtn.addEventListener('click', drawCard);
    answerInput.addEventListener('input', handleTyping);
    submitAnswerBtn.addEventListener('click', submitAnswer);
    continueBtn.addEventListener('click', nextQuestion);
    downloadResultsBtn.addEventListener('click', downloadResults);
    restartQuizBtn.addEventListener('click', restartQuiz);
    uploadResourceBtn.addEventListener('click', uploadResource);
    refreshScoresBtn.addEventListener('click', loadScores);
    downloadAllScoresBtn.addEventListener('click', downloadAllScores);
    
    // Initialize diamonds display
    initializeDiamonds();
    
    // Load resources and scores if available
    loadResources();
    loadScores();
}

// Initialize diamonds display
function initializeDiamonds() {
    diamondsDisplay.innerHTML = '';
    for (let i = 0; i < 10; i++) {
        const diamond = document.createElement('div');
        diamond.className = 'diamond';
        diamond.dataset.index = i;
        diamondsDisplay.appendChild(diamond);
    }
}

// Show admin login
function showAdminLogin(e) {
    e.preventDefault();
    loginScreen.classList.add('hidden');
    adminLoginScreen.classList.remove('hidden');
}

// Show user login
function showUserLogin() {
    adminLoginScreen.classList.add('hidden');
    loginScreen.classList.remove('hidden');
}

// Handle admin login
function handleAdminLogin() {
    const password = adminPasswordInput.value;
    
    if (password === adminPassword) {
        isAdmin = true;
        adminLoginScreen.classList.add('hidden');
        adminDashboard.classList.remove('hidden');
        
        // Load resources and scores
        loadResources();
        loadScores();
    } else {
        alert('Incorrect admin password. Please try again.');
    }
}

// Handle admin logout
function handleAdminLogout() {
    isAdmin = false;
    adminDashboard.classList.add('hidden');
    loginScreen.classList.remove('hidden');
    adminPasswordInput.value = '';
}

// Load resources from Firebase
function loadResources() {
    const resourcesRef = storage.ref('resources');
    
    resourcesRef.listAll().then((result) => {
        resourcesContainer.innerHTML = '';
        
        if (result.items.length === 0) {
            const noResources = document.createElement('li');
            noResources.textContent = 'No resources available';
            resourcesContainer.appendChild(noResources);
            return;
        }
        
        result.items.forEach((itemRef) => {
            itemRef.getMetadata().then((metadata) => {
                const resourceItem = document.createElement('li');
                const resourceLink = document.createElement('a');
                resourceLink.href = '#';
                resourceLink.textContent = metadata.name;
                resourceLink.addEventListener('click', (e) => {
                    e.preventDefault();
                    itemRef.getDownloadURL().then((url) => {
                        window.open(url, '_blank');
                    });
                });
                
                const deleteBtn = document.createElement('button');
                deleteBtn.textContent = 'Delete';
                deleteBtn.className = 'secondary-btn';
                deleteBtn.addEventListener('click', () => {
                    if (confirm(`Are you sure you want to delete ${metadata.name}?`)) {
                        itemRef.delete().then(() => {
                            loadResources();
                        }).catch((error) => {
                            console.error('Error deleting resource:', error);
                            alert('Error deleting resource. Please try again.');
                        });
                    }
                });
                
                resourceItem.appendChild(resourceLink);
                resourceItem.appendChild(deleteBtn);
                resourcesContainer.appendChild(resourceItem);
            });
        });
    }).catch((error) => {
        console.error('Error loading resources:', error);
        resourcesContainer.innerHTML = '<li>Error loading resources</li>';
    });
}

// Upload resource to Firebase
function uploadResource() {
    const file = resourceFileInput.files[0];
    
    if (!file) {
        alert('Please select a file to upload.');
        return;
    }
    
    const resourceRef = storage.ref(`resources/${file.name}`);
    
    // Upload file
    const uploadTask = resourceRef.put(file);
    
    uploadTask.on('state_changed', 
        (snapshot) => {
            // Progress function
            const progress = (snapshot.bytesTransferred / snapshot.totalBytes) * 100;
            console.log('Upload is ' + progress + '% done');
        }, 
        (error) => {
            // Error function
            console.error('Error uploading file:', error);
            alert('Error uploading file. Please try again.');
        }, 
        () => {
            // Complete function
            alert('File uploaded successfully!');
            resourceFileInput.value = '';
            loadResources();
        }
    );
}

// Load scores from Firebase
function loadScores() {
    const scoresRef = database.ref('scores');
    
    scoresRef.once('value').then((snapshot) => {
        const scores = snapshot.val();
        
        if (!scores) {
            scoresContainer.innerHTML = '<p>No scores available</p>';
            return;
        }
        
        let scoresHTML = '<table>';
        scoresHTML += '<tr><th>Name</th><th>Date</th><th>Score</th><th>Time</th><th>WPM</th><th>Rewards</th></tr>';
        
        Object.keys(scores).forEach((key) => {
            const score = scores[key];
            const date = new Date(score.endTime).toLocaleString();
            const totalMinutes = Math.floor(score.totalTime / 60);
            const totalSeconds = Math.round(score.totalTime % 60);
            const timeStr = `${totalMinutes}m ${totalSeconds}s`;
            
            // Get rewards list
            let rewardsStr = '';
            if (score.rewards && score.rewards.length > 0) {
                rewardsStr = score.rewards.join(', ');
            } else {
                rewardsStr = 'None';
            }
            
            scoresHTML += `<tr>
                <td>${score.username}</td>
                <td>${date}</td>
                <td>${score.scores.total}%</td>
                <td>${timeStr}</td>
                <td>${score.typingMetrics.averageWPM}</td>
                <td>${rewardsStr}</td>
            </tr>`;
        });
        
        scoresHTML += '</table>';
        scoresContainer.innerHTML = scoresHTML;
    }).catch((error) => {
        console.error('Error loading scores:', error);
        scoresContainer.innerHTML = '<p>Error loading scores</p>';
    });
}

// Download all scores
function downloadAllScores() {
    const scoresRef = database.ref('scores');
    
    scoresRef.once('value').then((snapshot) => {
        const scores = snapshot.val();
        
        if (!scores) {
            alert('No scores available to download.');
            return;
        }
        
        let scoresText = 'eClinicalWorks Quiz Scores\n';
        scoresText += `Date: ${new Date().toLocaleDateString()}\n\n`;
        scoresText += 'Name,Date,Total Score,Correctness,Clarity,Efficiency,Total Time,Avg WPM,Rewards\n';
        
        Object.keys(scores).forEach((key) => {
            const score = scores[key];
            const date = new Date(score.endTime).toLocaleString();
            const totalMinutes = Math.floor(score.totalTime / 60);
            const totalSeconds = Math.round(score.totalTime % 60);
            const timeStr = `${totalMinutes}m ${totalSeconds}s`;
            
            // Get rewards list
            let rewardsStr = '';
            if (score.rewards && score.rewards.length > 0) {
                rewardsStr = score.rewards.join('; ');
            }
            
            scoresText += `${score.username},${date},${score.scores.total}%,${score.scores.correctness}%,${score.scores.clarity}%,${score.scores.efficiency}%,${timeStr},${score.typingMetrics.averageWPM},"${rewardsStr}"\n`;
        });
        
        // Create a blob and download link
        const blob = new Blob([scoresText], { type: 'text/csv' });
        const url = URL.createObjectURL(blob);
        
        const a = document.createElement('a');
        a.href = url;
        a.download = `ecw_quiz_all_scores.csv`;
        document.body.appendChild(a);
        a.click();
        
        // Clean up
        setTimeout(() => {
            document.body.removeChild(a);
            URL.revokeObjectURL(url);
        }, 100);
    }).catch((error) => {
        console.error('Error downloading scores:', error);
        alert('Error downloading scores. Please try again.');
    });
}

// Handle login
function handleLogin() {
    if (!usernameInput.value.trim()) {
        alert('Please enter your name to continue.');
        return;
    }
    
    username = usernameInput.value.trim();
    userResults.username = username;
    userResults.startTime = new Date().toISOString();
    startTime = Date.now();
    
    userDisplay.textContent = username;
    
    // Hide login screen, show instructions
    loginScreen.classList.add('hidden');
    instructionsScreen.classList.remove('hidden');
}

// Start the quiz
function startQuiz() {
    // Hide instructions, show quiz
    instructionsScreen.classList.add('hidden');
    quizScreen.classList.remove('hidden');
    
    // Initialize quiz
    quizStartTime = Date.now();
    currentQuestionIndex = 0;
    userResults.questions = [];
    userResults.rewards = [];
    
    // Start the quiz timer
    startQuizTimer();
    
    // Update question counter
    updateQuestionCounter();
}

// Start the quiz timer
function startQuizTimer() {
    let timeRemaining = totalQuizTime;
    
    updateTimerDisplay(timeRemaining);
    
    quizTimer = setInterval(function() {
        timeRemaining--;
        
        if (timeRemaining <= 0) {
            clearInterval(quizTimer);
            endQuiz();
        }
        
        updateTimerDisplay(timeRemaining);
    }, 1000);
}

// Update timer display
function updateTimerDisplay(seconds) {
    const minutes = Math.floor(seconds / 60);
    const remainingSeconds = seconds % 60;
    
    timerDisplay.textContent = `${minutes.toString().padStart(2, '0')}:${remainingSeconds.toString().padStart(2, '0')}`;
}

// Update question counter
function updateQuestionCounter() {
    questionCounterDisplay.textContent = `${currentQuestionIndex + 1}/${questions.length}`;
}

// Draw a card
function drawCard() {
    // Flip the card
    card.classList.add('flipped');
    
    // Display the question
    const currentQuestion = questions[currentQuestionIndex];
    questionText.textContent = currentQuestion.text;
    
    // Disable draw button, enable answer input
    drawCardBtn.disabled = true;
    answerInput.disabled = false;
    
    // Start question timer
    questionStartTime = Date.now();
    startQuestionTimer();
    
    // Enable typing in the answer field
    answerInput.focus();
}

// Start question timer
function startQuestionTimer() {
    let questionTimeElapsed = 0;
    
    questionTimerDisplay.textContent = `Time: ${questionTimeElapsed}s`;
    
    questionTimer = setInterval(function() {
        questionTimeElapsed++;
        
        if (questionTimeElapsed >= questionTime) {
            clearInterval(questionTimer);
            submitAnswer();
        }
        
        questionTimerDisplay.textContent = `Time: ${questionTimeElapsed}s`;
    }, 1000);
}

// Handle typing in the answer field
function handleTyping() {
    // Enable submit button if there's text
    submitAnswerBtn.disabled = !answerInput.value.trim();
    
    // Start tracking typing if not already started
    if (!typingStartTime) {
        typingStartTime = Date.now();
        startTypingMetrics();
    }
    
    // Update typing metrics
    typingCharCount = answerInput.value.length;
    typingWordCount = answerInput.value.trim().split(/\s+/).length;
}

// Start tracking typing metrics
function startTypingMetrics() {
    typingTimer = setInterval(function() {
        const elapsedMinutes = (Date.now() - typingStartTime) / 60000;
        if (elapsedMinutes > 0) {
            wpm = Math.round(typingWordCount / elapsedMinutes);
            wpmDisplay.textContent = wpm;
        }
    }, 1000);
}

// Submit answer
function submitAnswer() {
    // Clear timers
    clearInterval(questionTimer);
    clearInterval(typingTimer);
    
    // Record end time
    questionEndTime = Date.now();
    typingEndTime = Date.now();
    
    // Calculate metrics
    const questionDuration = (questionEndTime - questionStartTime) / 1000;
    const typingDuration = (typingEndTime - typingStartTime) / 1000;
    
    // Get current question and user answer
    const currentQuestion = questions[currentQuestionIndex];
    const userAnswer = answerInput.value.trim();
    
    // Grade the answer
    const grades = gradeAnswer(currentQuestion, userAnswer);
    
    // Store results
    userResults.questions.push({
        questionId: currentQuestion.id,
        questionText: currentQuestion.text,
        userAnswer: userAnswer,
        correctAnswer: currentQuestion.correctAnswer,
        timeToAnswer: questionDuration,
        typingSpeed: wpm,
        grades: grades
    });
    
    // Update diamonds based on score
    if (grades.total >= 70) {
        diamondsCollected++;
        updateDiamonds();
    }
    
    // Calculate current score
    calculateCurrentScore();
    
    // Update landscape and rewards based on score
    updateLandscapeProgress();
    updateRewards();
    
    // Reset for next question
    resetQuestionState();
    
    // Show landscape screen
    quizScreen.classList.add('hidden');
    landscapeScreen.classList.remove('hidden');
}

// Calculate current score
function calculateCurrentScore() {
    if (userResults.questions.length === 0) return 0;
    
    let totalScore = 0;
    userResults.questions.forEach(q => {
        totalScore += q.grades.total;
    });
    
    currentScore = Math.round(totalScore / userResults.questions.length);
    return currentScore;
}

// Grade the answer
function gradeAnswer(question, userAnswer) {
    // This is a simplified grading algorithm
    // In a real application, this would be more sophisticated
    
    // Correctness (70%)
    let correctnessScore = 0;
    const correctAnswer = question.correctAnswer.toLowerCase();
    const userAnswerLower = userAnswer.toLowerCase();
    
    // Check for key terms and concepts
    const keyTerms = correctAnswer.split(/\s+/).filter(term => term.length > 4);
    const matchedTerms = keyTerms.filter(term => userAnswerLower.includes(term));
    
    correctnessScore = Math.min(100, Math.round((matchedTerms.length / keyTerms.length) * 100));
    
    // Clarity and organization (15%)
    let clarityScore = 0;
    
    // Check for numbered lists or bullet points
    if (userAnswer.match(/^\d+\.\s/m) || userAnswer.match(/^-\s/m) || userAnswer.match(/^\*\s/m)) {
        clarityScore += 50;
    }
    
    // Check for paragraphs and structure
    if (userAnswer.includes('\n\n')) {
        clarityScore += 25;
    }
    
    // Check for headings or sections
    if (userAnswer.match(/^[A-Z][^a-z]+:/m)) {
        clarityScore += 25;
    }
    
    clarityScore = Math.min(100, clarityScore);
    
    // Efficiency (15%)
    let efficiencyScore = 0;
    
    // WPM score (up to 50 points)
    efficiencyScore += Math.min(50, wpm);
    
    // Time efficiency (up to 30 points)
    const questionDuration = (questionEndTime - questionStartTime) / 1000;
    if (questionDuration < questionTime * 0.5) {
        efficiencyScore += 30;
    } else if (questionDuration < questionTime * 0.75) {
        efficiencyScore += 20;
    } else {
        efficiencyScore += 10;
    }
    
    // Grammar and punctuation (up to 20 points)
    // This is simplified - would use more sophisticated checks in a real app
    const sentences = userAnswer.split(/[.!?]+/).length - 1;
    const hasPunctuation = sentences > 0;
    const hasCapitalization = /[A-Z]/.test(userAnswer);
    
    if (hasPunctuation) efficiencyScore += 10;
    if (hasCapitalization) efficiencyScore += 10;
    
    efficiencyScore = Math.min(100, efficiencyScore);
    
    // Calculate total score
    const totalScore = Math.round(
        (correctnessScore * 0.7) +
        (clarityScore * 0.15) +
        (efficiencyScore * 0.15)
    );
    
    return {
        correctness: correctnessScore,
        clarity: clarityScore,
        efficiency: efficiencyScore,
        total: totalScore
    };
}

// Update diamonds display
function updateDiamonds() {
    diamondsCountDisplay.textContent = diamondsCollected;
    
    const diamonds = document.querySelectorAll('.diamond');
    for (let i = 0; i < diamonds.length; i++) {
        if (i < diamondsCollected) {
            diamonds[i].classList.add('collected');
        }
    }
}

// Update landscape progress based on score
function updateLandscapeProgress() {
    // Determine which phase we're in based on current score
    let phaseIndex = 0;
    
    if (currentScore >= 100) {
        phaseIndex = 4; // Phase 5 (100%)
    } else if (currentScore >= 80) {
        phaseIndex = 3; // Phase 4 (80%)
    } else if (currentScore >= 60) {
        phaseIndex = 2; // Phase 3 (60%)
    } else if (currentScore >= 40) {
        phaseIndex = 1; // Phase 2 (40%)
    } else if (currentScore >= 20) {
        phaseIndex = 0; // Phase 1 (20%)
    } else {
        return; // Not enough score yet
    }
    
    // Update landscape elements for all phases up to current phase
    for (let i = 0; i <= phaseIndex; i++) {
        const phase = landscapePhases[i];
        phase.elements.forEach(element => {
            const el = document.querySelector(element.selector);
            if (el) {
                if (element.action === 'show') {
                    el.classList.remove('hidden');
                } else if (element.action === 'hide') {
                    el.classList.add('hidden');
                } else if (element.action === 'addClass') {
                    el.classList.add(element.class);
                } else if (element.action === 'removeClass') {
                    el.classList.remove(element.class);
                }
            }
        });
    }
    
    // Update user name on flag if in phase 5
    if (phaseIndex === 4) {
        const userNameDisplay = document.querySelector('.user-name-display');
        if (userNameDisplay) {
            userNameDisplay.textContent = username;
        }
    }
}

// Update rewards based on score
function updateRewards() {
    // Check each reward threshold
    rewardThresholds.forEach(reward => {
        if (currentScore >= reward.score) {
            // Check if we already have this reward
            if (!userResults.rewards.includes(reward.name)) {
                userResults.rewards.push(reward.name);
            }
            
            // Update the UI
            const rewardElement = document.querySelector(`.reward.${reward.class}`);
            if (rewardElement) {
                rewardElement.classList.remove('hidden');
                rewardElement.classList.add('unlocked');
            }
        }
    });
}

// Reset question state
function resetQuestionState() {
    // Reset card
    card.classList.remove('flipped');
    
    // Reset buttons and inputs
    drawCardBtn.disabled = false;
    answerInput.disabled = true;
    submitAnswerBtn.disabled = true;
    
    // Clear answer input
    answerInput.value = '';
    
    // Reset typing metrics
    typingStartTime = 0;
    typingEndTime = 0;
    typingCharCount = 0;
    typingWordCount = 0;
    wpm = 0;
    wpmDisplay.textContent = '0';
}

// Move to next question
function nextQuestion() {
    currentQuestionIndex++;
    
    if (currentQuestionIndex >= questions.length) {
        // End of quiz
        endQuiz();
    } else {
        // Update question counter
        updateQuestionCounter();
        
        // Show quiz screen
        landscapeScreen.classList.add('hidden');
        quizScreen.classList.remove('hidden');
    }
}

// End the quiz
function endQuiz() {
    // Clear timers
    clearInterval(quizTimer);
    clearInterval(questionTimer);
    clearInterval(typingTimer);
    
    // Record end time
    userResults.endTime = new Date().toISOString();
    userResults.totalTime = (Date.now() - quizStartTime) / 1000;
    
    // Calculate final scores
    calculateFinalScores();
    
    // Save results to Firebase
    saveResultsToFirebase();
    
    // Display results
    displayResults();
    
    // Hide all screens except results
    loginScreen.classList.add('hidden');
    instructionsScreen.classList.add('hidden');
    quizScreen.classList.add('hidden');
    landscapeScreen.classList.add('hidden');
    resultsScreen.classList.remove('hidden');
}

// Save results to Firebase
function saveResultsToFirebase() {
    const resultsRef = database.ref('scores').push();
    resultsRef.set(userResults)
        .catch(error => {
            console.error("Error saving results to Firebase:", error);
        });
}

// Calculate final scores
function calculateFinalScores() {
    if (userResults.questions.length === 0) return;
    
    let totalCorrectness = 0;
    let totalClarity = 0;
    let totalEfficiency = 0;
    let totalWPM = 0;
    let totalChars = 0;
    let totalWords = 0;
    
    userResults.questions.forEach(q => {
        totalCorrectness += q.grades.correctness;
        totalClarity += q.grades.clarity;
        totalEfficiency += q.grades.efficiency;
        totalWPM += q.typingSpeed || 0;
        totalChars += q.userAnswer.length;
        totalWords += q.userAnswer.split(/\s+/).length;
    });
    
    userResults.scores.correctness = Math.round(totalCorrectness / userResults.questions.length);
    userResults.scores.clarity = Math.round(totalClarity / userResults.questions.length);
    userResults.scores.efficiency = Math.round(totalEfficiency / userResults.questions.length);
    userResults.scores.total = Math.round(
        (userResults.scores.correctness * 0.7) +
        (userResults.scores.clarity * 0.15) +
        (userResults.scores.efficiency * 0.15)
    );
    
    userResults.typingMetrics.averageWPM = Math.round(totalWPM / userResults.questions.length);
    userResults.typingMetrics.totalCharacters = totalChars;
    userResults.typingMetrics.totalWords = totalWords;
    
    // Final update of rewards based on final score
    currentScore = userResults.scores.total;
    updateRewards();
}

// Display results
function displayResults() {
    // Display user name
    document.getElementById('result-user-name').textContent = username;
    
    // Display scores
    document.getElementById('total-score').textContent = userResults.scores.total;
    document.getElementById('correctness-score').textContent = userResults.scores.correctness;
    document.getElementById('clarity-score').textContent = userResults.scores.clarity;
    document.getElementById('efficiency-score').textContent = userResults.scores.efficiency;
    
    // Display time metrics
    const totalMinutes = Math.floor(userResults.totalTime / 60);
    const totalSeconds = Math.round(userResults.totalTime % 60);
    document.getElementById('total-time').textContent = `${totalMinutes}m ${totalSeconds}s`;
    
    const avgTime = userResults.totalTime / userResults.questions.length;
    const avgMinutes = Math.floor(avgTime / 60);
    const avgSeconds = Math.round(avgTime % 60);
    document.getElementById('avg-time').textContent = `${avgMinutes}m ${avgSeconds}s`;
    
    document.getElementById('avg-wpm').textContent = userResults.typingMetrics.averageWPM;
    
    // Display final rewards
    displayFinalRewards();
    
    // Clone final landscape
    const finalLandscapeDisplay = document.getElementById('final-landscape-display');
    finalLandscapeDisplay.innerHTML = '';
    const finalLandscape = document.getElementById('landscape').cloneNode(true);
    finalLandscapeDisplay.appendChild(finalLandscape);
    
    // Display detailed question analysis
    const questionsReview = document.getElementById('questions-review');
    questionsReview.innerHTML = '';
    
    userResults.questions.forEach((q, index) => {
        const questionItem = document.createElement('div');
        questionItem.className = 'question-review-item';
        
        questionItem.innerHTML = `
            <h4>Question ${index + 1}</h4>
            <p><strong>Question:</strong> ${q.questionText}</p>
            <p><strong>Your Answer:</strong> ${q.userAnswer}</p>
            <p><strong>Correct Answer:</strong> ${q.correctAnswer}</p>
            <div class="scores">
                <span>Correctness: ${q.grades.correctness}%</span>
                <span>Clarity: ${q.grades.clarity}%</span>
                <span>Efficiency: ${q.grades.efficiency}%</span>
                <span>Total: ${q.grades.total}%</span>
            </div>
            <p><strong>Time to Answer:</strong> ${Math.round(q.timeToAnswer)}s</p>
            <p><strong>Typing Speed:</strong> ${q.typingSpeed || 0} WPM</p>
        `;
        
        questionsReview.appendChild(questionItem);
    });
    
    // Generate improvement suggestions
    generateSuggestions();
}

// Display final rewards
function displayFinalRewards() {
    const finalRewardsDisplay = document.getElementById('final-rewards-display');
    finalRewardsDisplay.innerHTML = '';
    
    if (userResults.rewards.length === 0) {
        finalRewardsDisplay.innerHTML = '<p>No rewards earned yet. Keep practicing!</p>';
        return;
    }
    
    // Display each earned reward
    rewardThresholds.forEach(reward => {
        if (userResults.rewards.includes(reward.name)) {
            const rewardElement = document.createElement('div');
            rewardElement.className = `reward ${reward.class} unlocked`;
            
            rewardElement.innerHTML = `
                <img src="assets/images/${reward.image}" alt="${reward.name}">
                <span>${reward.name}</span>
            `;
            
            finalRewardsDisplay.appendChild(rewardElement);
        }
    });
}

// Generate improvement suggestions
function generateSuggestions() {
    const suggestions = document.getElementById('suggestions');
    suggestions.innerHTML = '';
    
    const suggestionsList = [];
    
    // Correctness suggestions
    if (userResults.scores.correctness < 70) {
        suggestionsList.push('Review the eClinicalWorks documentation to improve your knowledge of the system.');
        suggestionsList.push('Focus on understanding the key steps in each process.');
        suggestionsList.push('Practice with real-world scenarios in a test environment.');
    }
    
    // Clarity suggestions
    if (userResults.scores.clarity < 70) {
        suggestionsList.push('Use numbered lists or bullet points to organize your answers.');
        suggestionsList.push('Break down complex processes into clear, sequential steps.');
        suggestionsList.push('Use headings or sections to structure longer answers.');
    }
    
    // Efficiency suggestions
    if (userResults.scores.efficiency < 70) {
        suggestionsList.push('Practice typing to improve your speed and accuracy.');
        suggestionsList.push('Work on time management to complete questions more efficiently.');
        suggestionsList.push('Pay attention to grammar and punctuation in your responses.');
    }
    
    // Add general suggestions
    suggestionsList.push('Review questions you scored lowest on for targeted improvement.');
    suggestionsList.push('Practice regularly to build muscle memory for common eCW tasks.');
    
    // Display suggestions
    suggestionsList.forEach(suggestion => {
        const suggestionItem = document.createElement('div');
        suggestionItem.className = 'suggestion-item';
        suggestionItem.textContent = suggestion;
        suggestions.appendChild(suggestionItem);
    });
}

// Download results
function downloadResults() {
    // Create a text representation of the results
    let resultsText = `eClinicalWorks Quiz Results for ${username}\n`;
    resultsText += `Date: ${new Date().toLocaleDateString()}\n\n`;
    
    resultsText += `SUMMARY\n`;
    resultsText += `========\n`;
    resultsText += `Total Score: ${userResults.scores.total}%\n`;
    resultsText += `Correctness: ${userResults.scores.correctness}%\n`;
    resultsText += `Clarity & Organization: ${userResults.scores.clarity}%\n`;
    resultsText += `Speed & Efficiency: ${userResults.scores.efficiency}%\n\n`;
    
    resultsText += `Time Metrics\n`;
    resultsText += `-----------\n`;
    const totalMinutes = Math.floor(userResults.totalTime / 60);
    const totalSeconds = Math.round(userResults.totalTime % 60);
    resultsText += `Total Time: ${totalMinutes}m ${totalSeconds}s\n`;
    
    const avgTime = userResults.totalTime / userResults.questions.length;
    const avgMinutes = Math.floor(avgTime / 60);
    const avgSeconds = Math.round(avgTime % 60);
    resultsText += `Average Time per Question: ${avgMinutes}m ${avgSeconds}s\n`;
    resultsText += `Average WPM: ${userResults.typingMetrics.averageWPM}\n\n`;
    
    resultsText += `Rewards Earned\n`;
    resultsText += `-------------\n`;
    if (userResults.rewards.length > 0) {
        userResults.rewards.forEach(reward => {
            resultsText += `- ${reward}\n`;
        });
    } else {
        resultsText += `None\n`;
    }
    resultsText += `\n`;
    
    resultsText += `DETAILED QUESTION ANALYSIS\n`;
    resultsText += `=========================\n\n`;
    
    userResults.questions.forEach((q, index) => {
        resultsText += `Question ${index + 1}\n`;
        resultsText += `----------\n`;
        resultsText += `Question: ${q.questionText}\n\n`;
        resultsText += `Your Answer: ${q.userAnswer}\n\n`;
        resultsText += `Correct Answer: ${q.correctAnswer}\n\n`;
        resultsText += `Scores: Correctness: ${q.grades.correctness}%, Clarity: ${q.grades.clarity}%, Efficiency: ${q.grades.efficiency}%, Total: ${q.grades.total}%\n`;
        resultsText += `Time to Answer: ${Math.round(q.timeToAnswer)}s\n`;
        resultsText += `Typing Speed: ${q.typingSpeed || 0} WPM\n\n`;
    });
    
    resultsText += `SUGGESTIONS FOR IMPROVEMENT\n`;
    resultsText += `===========================\n`;
    
    const suggestionItems = document.querySelectorAll('.suggestion-item');
    suggestionItems.forEach((item, index) => {
        resultsText += `${index + 1}. ${item.textContent}\n`;
    });
    
    // Create a blob and download link
    const blob = new Blob([resultsText], { type: 'text/plain' });
    const url = URL.createObjectURL(blob);
    
    const a = document.createElement('a');
    a.href = url;
    a.download = `ecw_quiz_results_${username.replace(/\s+/g, '_')}.txt`;
    document.body.appendChild(a);
    a.click();
    
    // Clean up
    setTimeout(() => {
        document.body.removeChild(a);
        URL.revokeObjectURL(url);
    }, 100);
}

// Restart quiz
function restartQuiz() {
    // Reset all state
    username = '';
    startTime = 0;
    quizStartTime = 0;
    questionStartTime = 0;
    questionEndTime = 0;
    currentQuestionIndex = 0;
    diamondsCollected = 0;
    landscapeProgress = 0;
    currentScore = 0;
    userResults = {
        username: '',
        startTime: '',
        endTime: '',
        totalTime: 0,
        questions: [],
        scores: {
            correctness: 0,
            clarity: 0,
            efficiency: 0,
            total: 0
        },
        typingMetrics: {
            averageWPM: 0,
            totalCharacters: 0,
            totalWords: 0
        },
        rewards: []
    };
    
    // Reset UI
    card.classList.remove('flipped');
    initializeDiamonds();
    
    // Reset rewards
    const rewardElements = document.querySelectorAll('.reward');
    rewardElements.forEach(el => {
        el.classList.add('hidden');
        el.classList.remove('unlocked');
    });
    
    // Reset landscape elements
    const landscapeElements = document.querySelectorAll('.landscape > div:not(.sky):not(.mountains):not(.ground)');
    landscapeElements.forEach(el => {
        el.classList.add('hidden');
    });
    
    // Show login screen
    resultsScreen.classList.add('hidden');
    loginScreen.classList.remove('hidden');
    
    // Clear user input
    usernameInput.value = '';
}