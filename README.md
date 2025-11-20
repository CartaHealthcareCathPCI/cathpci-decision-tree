<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NCDR CathPCI - Cath Lab Indication Decision Tree</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            background: white;
            border-radius: 12px;
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.2);
            overflow: hidden;
        }
        
        .header {
            background: linear-gradient(135deg, #2c3e50 0%, #34495e 100%);
            color: white;
            padding: 30px;
            text-align: center;
        }
        
        .header h1 {
            font-size: 28px;
            margin-bottom: 10px;
        }
        
        .header p {
            font-size: 14px;
            opacity: 0.9;
        }
        
        .content {
            padding: 30px;
        }
        
        .step-section {
            background: #f8f9fa;
            border-left: 4px solid #667eea;
            border-radius: 8px;
            padding: 25px;
            margin-bottom: 25px;
            transition: all 0.3s ease;
        }
        
        .step-section:hover {
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        }
        
        .step-number {
            display: inline-block;
            background: #667eea;
            color: white;
            width: 32px;
            height: 32px;
            border-radius: 50%;
            text-align: center;
            line-height: 32px;
            font-weight: bold;
            margin-right: 10px;
            font-size: 16px;
        }
        
        .step-title {
            font-size: 18px;
            font-weight: 600;
            color: #2c3e50;
            margin-bottom: 15px;
            display: flex;
            align-items: center;
        }
        
        .option-group {
            margin-left: 42px;
            margin-top: 15px;
        }
        
        .option {
            background: white;
            border: 2px solid #e0e0e0;
            border-radius: 6px;
            padding: 12px 15px;
            margin-bottom: 10px;
            cursor: pointer;
            transition: all 0.2s ease;
            display: flex;
            align-items: center;
        }
        
        .option:hover {
            border-color: #667eea;
            background: #f0f4ff;
        }
        
        .option.selected {
            border-color: #667eea;
            background: #e8edff;
            font-weight: 600;
        }
        
        .option input[type="checkbox"] {
            margin-right: 12px;
            width: 20px;
            height: 20px;
            cursor: pointer;
        }
        
        .option input[type="radio"] {
            margin-right: 12px;
            width: 20px;
            height: 20px;
            cursor: pointer;
        }
        
        .sub-option-group {
            margin-left: 35px;
            margin-top: 10px;
            padding-left: 15px;
            border-left: 2px solid #d0d0d0;
        }
        
        .reminder {
            background: #fff3cd;
            border-left: 4px solid #ffc107;
            padding: 12px 15px;
            margin: 10px 0 10px 42px;
            border-radius: 4px;
            font-size: 14px;
            color: #856404;
        }
        
        .reminder strong {
            display: block;
            margin-bottom: 5px;
        }
        
        .validation-error {
            background: #f8d7da;
            border-left: 4px solid #dc3545;
            padding: 12px 15px;
            margin: 10px 0;
            border-radius: 4px;
            font-size: 14px;
            color: #721c24;
        }
        
        .summary-section {
            background: #e8f5e9;
            border: 2px solid #4caf50;
            border-radius: 8px;
            padding: 25px;
            margin-top: 30px;
        }
        
        .summary-title {
            font-size: 20px;
            font-weight: 600;
            color: #2e7d32;
            margin-bottom: 15px;
            display: flex;
            align-items: center;
        }
        
        .summary-title::before {
            content: "✓";
            display: inline-block;
            background: #4caf50;
            color: white;
            width: 28px;
            height: 28px;
            border-radius: 50%;
            text-align: center;
            line-height: 28px;
            margin-right: 10px;
            font-size: 18px;
        }
        
        .summary-content {
            background: white;
            padding: 15px;
            border-radius: 6px;
            margin-bottom: 15px;
        }
        
        .summary-label {
            font-weight: 600;
            color: #2c3e50;
            margin-bottom: 8px;
        }
        
        .indication-list {
            list-style: none;
            padding-left: 0;
        }
        
        .indication-list li {
            padding: 8px 12px;
            background: #f8f9fa;
            border-left: 3px solid #667eea;
            margin-bottom: 5px;
            border-radius: 3px;
        }
        
        .pci-indication {
            background: #fff3e0;
            border: 2px solid #ff9800;
            padding: 15px;
            border-radius: 6px;
            margin-top: 15px;
        }
        
        .pci-indication-label {
            font-weight: 600;
            color: #e65100;
            margin-bottom: 8px;
        }
        
        .pci-indication-value {
            font-size: 16px;
            color: #2c3e50;
            font-weight: 600;
        }
        
        .reset-button {
            background: #dc3545;
            color: white;
            border: none;
            padding: 12px 24px;
            border-radius: 6px;
            cursor: pointer;
            font-size: 16px;
            font-weight: 600;
            margin-top: 20px;
            transition: all 0.2s ease;
        }
        
        .reset-button:hover {
            background: #c82333;
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }
        
        .instruction {
            color: #666;
            font-size: 14px;
            font-style: italic;
            margin-left: 42px;
            margin-top: 5px;
        }
        
        .no-selection {
            color: #999;
            font-style: italic;
        }
        
        .pci-sub-question {
            background: #fff9e6;
            border: 2px solid #ffc107;
            border-radius: 6px;
            padding: 20px;
            margin-top: 15px;
        }
        
        .pci-sub-question-title {
            font-weight: 600;
            color: #856404;
            margin-bottom: 15px;
            font-size: 16px;
        }
        
        .hidden {
            display: none;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>NCDR CathPCI Decision Tree</h1>
            <p>Cath Lab Indication & PCI Indication Selection Tool</p>
        </div>
        
        <div class="content">
            <!-- Step 1: ACS -->
            <div class="step-section">
                <div class="step-title">
                    <span class="step-number">1</span>
                    Is the patient experiencing ACS (Acute Coronary Syndrome) – STEMI/NSTEMI/Unstable Angina?
                </div>
                <div class="option-group">
                    <div class="option" onclick="toggleRadioGroup('acs', 'yes')">
                        <input type="radio" name="acs" id="acs-yes" value="yes">
                        <label for="acs-yes">Yes - Select timing below:</label>
                    </div>
                    <div class="sub-option-group" id="acs-timing-group" style="display: none;">
                        <div class="option" onclick="selectIndication('ACS <= 24 hrs', 'acs-24')">
                            <input type="radio" name="acs-timing" id="acs-24" value="ACS <= 24 hrs">
                            <label for="acs-24">ACS ≤ 24 hrs</label>
                        </div>
                        <div class="option" onclick="selectIndication('ACS > 24 hrs', 'acs-24plus')">
                            <input type="radio" name="acs-timing" id="acs-24plus" value="ACS > 24 hrs">
                            <label for="acs-24plus">ACS > 24 hrs</label>
                        </div>
                    </div>
                    <div class="option" onclick="toggleRadioGroup('acs', 'no')">
                        <input type="radio" name="acs" id="acs-no" value="no">
                        <label for="acs-no">No - Move to next step</label>
                    </div>
                </div>
            </div>

            <!-- Step 2: Angina -->
            <div class="step-section">
                <div class="step-title">
                    <span class="step-number">2</span>
                    Is the patient being brought to the Cath Lab because they are experiencing Angina or a documented Anginal Equivalent?
                </div>
                <div class="option-group">
                    <div class="option" onclick="toggleRadioGroup('angina', 'yes')">
                        <input type="radio" name="angina" id="angina-yes" value="yes">
                        <label for="angina-yes">Yes - Consider patient history:</label>
                    </div>
                    <div class="sub-option-group" id="angina-history-group" style="display: none;">
                        <p style="margin-bottom: 10px; font-weight: 600; color: #2c3e50;">Does the patient have a history of: MI/PCI/CABG/Chest Pain > 2 months?</p>
                        <div class="option" onclick="selectIndication('Worsening Angina', 'worsening-angina')">
                            <input type="radio" name="angina-type" id="worsening-angina" value="Worsening Angina">
                            <label for="worsening-angina">Yes - Worsening Angina</label>
                        </div>
                        <div class="option" onclick="selectIndication('New Onset Angina <= 2 months', 'new-onset-angina')">
                            <input type="radio" name="angina-type" id="new-onset-angina" value="New Onset Angina <= 2 months">
                            <label for="new-onset-angina">No - New Onset Angina ≤ 2 months</label>
                        </div>
                    </div>
                    <div class="option" onclick="toggleRadioGroup('angina', 'no')">
                        <input type="radio" name="angina" id="angina-no" value="no">
                        <label for="angina-no">No - Move to next step</label>
                    </div>
                </div>
            </div>

            <!-- Step 3: CAD History -->
            <div class="step-section">
                <div class="step-title">
                    <span class="step-number">3</span>
                    Does the patient have a history of CAD (Coronary Artery Disease)?
                </div>
                <div class="option-group">
                    <div class="option" onclick="toggleRadioGroup('cad-history', 'yes')">
                        <input type="radio" name="cad-history" id="cad-yes" value="yes">
                        <label for="cad-yes">Yes - Consider stability:</label>
                    </div>
                    <div class="sub-option-group" id="cad-stability-group" style="display: none;">
                        <p style="margin-bottom: 10px; font-weight: 600; color: #2c3e50;">Is the patient CURRENTLY stable (no signs/symptoms of ACS, New Onset or Worsening Angina, or hemodynamic instability) AND has known CAD ≥50% in at least one vessel?</p>
                        <div class="option" onclick="selectIndication('Stable Known CAD', 'stable-cad')">
                            <input type="radio" name="cad-stable" id="stable-cad" value="Stable Known CAD">
                            <label for="stable-cad">Yes - Stable Known CAD</label>
                        </div>
                        <div class="option" onclick="toggleRadioGroup('cad-stable', 'no')">
                            <input type="radio" name="cad-stable" id="cad-stable-no" value="no">
                            <label for="cad-stable-no">No - Do not select</label>
                        </div>
                    </div>
                    <div class="option" onclick="toggleRadioGroup('cad-history', 'no')">
                        <input type="radio" name="cad-history" id="cad-no" value="no">
                        <label for="cad-no">No - Consider Suspected CAD:</label>
                    </div>
                    <div class="sub-option-group" id="suspected-cad-group" style="display: none;">
                        <div class="option" onclick="toggleCheckbox('suspected-cad-cb')">
                            <input type="checkbox" id="suspected-cad-cb" onchange="handleCheckboxChange('Suspected CAD', this)">
                            <label for="suspected-cad-cb">Suspected CAD (if applicable)</label>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Step 4: Cardiac Arrest -->
            <div class="step-section">
                <div class="step-title">
                    <span class="step-number">4</span>
                    Is the patient being brought to the Cath Lab following a Cardiac Arrest?
                </div>
                <div class="option-group">
                    <div class="option" onclick="toggleCheckbox('cardiac-arrest')">
                        <input type="checkbox" id="cardiac-arrest" onchange="handleCheckboxChange('Resuscitated Cardiac Arrest', this)">
                        <label for="cardiac-arrest">Yes - Resuscitated Cardiac Arrest</label>
                    </div>
                    <div class="option">
                        <input type="radio" name="cardiac-arrest-group" checked disabled style="visibility: hidden;">
                        <label>No - Move to next step</label>
                    </div>
                </div>
            </div>

            <!-- Step 5: Non-Ischemic/Structural -->
            <div class="step-section">
                <div class="step-title">
                    <span class="step-number">5</span>
                    Is this Cath Lab visit being prompted by a Non-Ischemic/Structural Issue?
                </div>
                <p class="instruction">Select all that apply:</p>
                <div class="option-group">
                    <div class="option" onclick="toggleCheckbox('valvular')">
                        <input type="checkbox" id="valvular" onchange="handleCheckboxChange('Valvular Disease', this)">
                        <label for="valvular">Valvular Disease</label>
                    </div>
                    <div class="reminder" id="valvular-reminder" style="display: none;">
                        <strong>⚠️ Reminder:</strong>
                        Include ALL stenosis and regurgitation noted on the most recent echo (within 6 months), not just the vessel prompting the cath lab visit.
                    </div>
                    
                    <div class="option" onclick="toggleCheckbox('pericardial')">
                        <input type="checkbox" id="pericardial" onchange="handleCheckboxChange('Pericardial Disease', this)">
                        <label for="pericardial">Pericardial Disease</label>
                    </div>
                    
                    <div class="option" onclick="toggleCheckbox('arrhythmia')">
                        <input type="checkbox" id="arrhythmia" onchange="handleCheckboxChange('Cardiac arrhythmia', this)">
                        <label for="arrhythmia">Cardiac Arrhythmia</label>
                    </div>
                    <div class="reminder" id="arrhythmia-reminder" style="display: none;">
                        <strong>⚠️ Reminder:</strong>
                        A HISTORY ONLY of arrhythmia would not allow for coding this - it must be a reason that is prompting the current cath lab visit.
                    </div>
                    
                    <div class="option" onclick="toggleCheckbox('cardiomyopathy')">
                        <input type="checkbox" id="cardiomyopathy" onchange="handleCheckboxChange('Cardiomyopathy', this)">
                        <label for="cardiomyopathy">Cardiomyopathy</label>
                    </div>
                    <div class="reminder" id="cardiomyopathy-reminder" style="display: none;">
                        <strong>⚠️ Reminder:</strong>
                        Cardiomyopathy must be explicitly stated as an indication by the physician.
                    </div>
                    
                    <div class="option" onclick="toggleCheckbox('lv-dysfunction')">
                        <input type="checkbox" id="lv-dysfunction" onchange="handleCheckboxChange('LV Dysfunction', this)">
                        <label for="lv-dysfunction">LV Dysfunction</label>
                    </div>
                    <div class="reminder" id="lv-dysfunction-reminder" style="display: none;">
                        <strong>⚠️ Reminder:</strong>
                        This indication is the best fit for patients being brought to the Cath Lab for Heart Failure.
                    </div>
                    
                    <div class="option" onclick="toggleCheckbox('syncope')">
                        <input type="checkbox" id="syncope" onchange="handleCheckboxChange('Syncope', this)">
                        <label for="syncope">Syncope</label>
                    </div>
                    <div class="reminder" id="syncope-reminder" style="display: none;">
                        <strong>⚠️ Reminder:</strong>
                        A HISTORY ONLY of syncope would not allow for coding this - it must be a reason that is prompting the current cath lab visit.
                    </div>
                    
                    <div class="option" onclick="toggleCheckbox('post-transplant')">
                        <input type="checkbox" id="post-transplant" onchange="handleCheckboxChange('Post Cardiac Transplant', this)">
                        <label for="post-transplant">Post Cardiac Transplant</label>
                    </div>
                </div>
            </div>

            <!-- Step 6: Non-Cardiac/Evaluation -->
            <div class="step-section">
                <div class="step-title">
                    <span class="step-number">6</span>
                    Is the patient being seen in the Cath Lab for a Non-Cardiac/Evaluation reason?
                </div>
                <p class="instruction">Select all that apply:</p>
                <div class="option-group">
                    <div class="option" onclick="toggleCheckbox('pre-op')">
                        <input type="checkbox" id="pre-op" onchange="handleCheckboxChange('Pre-operative Evaluation', this)">
                        <label for="pre-op">Pre-Operative Evaluation</label>
                    </div>
                    <div class="reminder" id="pre-op-reminder" style="display: none;">
                        <strong>⚠️ Reminder:</strong>
                        TAVR evaluations should be captured with the Valvular Disease indication, not Pre-Operative Evaluation.
                    </div>
                    
                    <div class="option" onclick="toggleCheckbox('exercise')">
                        <input type="checkbox" id="exercise" onchange="handleCheckboxChange('Evaluation for Exercise Clearance', this)">
                        <label for="exercise">Evaluation for Exercise Clearance</label>
                    </div>
                </div>
            </div>

            <!-- Step 7: Other -->
            <div class="step-section">
                <div class="step-title">
                    <span class="step-number">7</span>
                    If NO OTHER INDICATIONS apply to this Cath Lab scenario:
                </div>
                <div class="option-group">
                    <div class="option" onclick="toggleCheckbox('other')">
                        <input type="checkbox" id="other" onchange="handleCheckboxChange('Other', this)">
                        <label for="other">Other</label>
                    </div>
                    <div class="reminder" id="other-reminder" style="display: none;">
                        <strong>⚠️ Reminder:</strong>
                        Other should be used very rarely and ONLY when no other indications apply. If the only reason listed for Cath Lab Indication is "Abnormal Stress Test", this does NOT support coding Other. The abstractor should review the patient condition prompting the Stress Test for more information.
                    </div>
                </div>
            </div>

            <!-- Validation Errors -->
            <div id="validation-errors"></div>

            <!-- Summary Section -->
            <div class="summary-section">
                <div class="summary-title">Selected Indications Summary</div>
                
                <div class="summary-content">
                    <div class="summary-label">Cath Lab Indications:</div>
                    <ul class="indication-list" id="selected-indications">
                        <li class="no-selection">No indications selected yet</li>
                    </ul>
                </div>

                <div id="pci-indication-section" class="hidden">
                    <!-- PCI sub-questions will appear here when needed -->
                </div>

                <div class="pci-indication">
                    <div class="pci-indication-label">PCI Indication:</div>
                    <div class="pci-indication-value" id="pci-indication">
                        <span class="no-selection">Will be determined based on Cath Lab Indications</span>
                    </div>
                </div>

                <button class="reset-button" onclick="resetForm()">Reset All Selections</button>
            </div>
        </div>
    </div>

    <script>
        // Store selected indications
        let selectedIndications = new Set();
        let pciSubAnswers = {};

        // Indication priority order (highest to lowest)
        const priorityOrder = [
            'ACS <= 24 hrs',
            'ACS > 24 hrs',
            'New Onset Angina <= 2 months',
            'Stable Known CAD',
            'Suspected CAD',
            'Worsening Angina',
            'Resuscitated Cardiac Arrest',
            'Valvular Disease',
            'Pericardial Disease',
            'Cardiac arrhythmia',
            'Cardiomyopathy',
            'LV Dysfunction',
            'Syncope',
            'Post Cardiac Transplant',
            'Pre-operative Evaluation',
            'Evaluation for Exercise Clearance',
            'Other'
        ];

        function toggleRadioGroup(groupName, value) {
            const radio = document.querySelector(`input[name="${groupName}"][value="${value}"]`);
            if (radio) {
                radio.checked = true;
                handleRadioChange(groupName, value);
            }
        }

        function handleRadioChange(groupName, value) {
            // Handle ACS radio group
            if (groupName === 'acs') {
                const timingGroup = document.getElementById('acs-timing-group');
                if (value === 'yes') {
                    timingGroup.style.display = 'block';
                } else {
                    timingGroup.style.display = 'none';
                    // Clear ACS selections
                    document.getElementById('acs-24').checked = false;
                    document.getElementById('acs-24plus').checked = false;
                    removeIndication('ACS <= 24 hrs');
                    removeIndication('ACS > 24 hrs');
                }
            }

            // Handle Angina radio group
            if (groupName === 'angina') {
                const historyGroup = document.getElementById('angina-history-group');
                if (value === 'yes') {
                    historyGroup.style.display = 'block';
                } else {
                    historyGroup.style.display = 'none';
                    // Clear angina selections
                    document.getElementById('worsening-angina').checked = false;
                    document.getElementById('new-onset-angina').checked = false;
                    removeIndication('Worsening Angina');
                    removeIndication('New Onset Angina <= 2 months');
                }
            }

            // Handle CAD history radio group
            if (groupName === 'cad-history') {
                const stabilityGroup = document.getElementById('cad-stability-group');
                const suspectedGroup = document.getElementById('suspected-cad-group');
                if (value === 'yes') {
                    stabilityGroup.style.display = 'block';
                    suspectedGroup.style.display = 'none';
                    document.getElementById('suspected-cad-cb').checked = false;
                    removeIndication('Suspected CAD');
                } else {
                    stabilityGroup.style.display = 'none';
                    suspectedGroup.style.display = 'block';
                    document.getElementById('stable-cad').checked = false;
                    removeIndication('Stable Known CAD');
                }
            }

            // Handle CAD stable radio group
            if (groupName === 'cad-stable') {
                if (value === 'no') {
                    document.getElementById('stable-cad').checked = false;
                    removeIndication('Stable Known CAD');
                }
            }
        }

        function toggleCheckbox(checkboxId) {
            const checkbox = document.getElementById(checkboxId);
            checkbox.checked = !checkbox.checked;
            checkbox.dispatchEvent(new Event('change'));
        }

        function handleCheckboxChange(indication, checkbox) {
            if (checkbox.checked) {
                selectIndication(indication, checkbox.id);
            } else {
                removeIndication(indication);
            }
        }

        function selectIndication(indication, elementId) {
            // Check if this is a radio button selection
            const element = document.getElementById(elementId);
            if (element && element.type === 'radio') {
                // For radio buttons in the same group, remove other selections
                const radioName = element.name;
                const radios = document.querySelectorAll(`input[name="${radioName}"]`);
                radios.forEach(radio => {
                    if (radio.id !== elementId && radio.value) {
                        const val = radio.value;
                        if (priorityOrder.includes(val)) {
                            removeIndication(val);
                        }
                    }
                });
            }

            selectedIndications.add(indication);
            showReminder(indication);
            updateSummary();
            validateSelections();
        }

        function removeIndication(indication) {
            selectedIndications.delete(indication);
            hideReminder(indication);
            updateSummary();
            validateSelections();
        }

        function showReminder(indication) {
            const reminderMap = {
                'Valvular Disease': 'valvular-reminder',
                'Cardiac arrhythmia': 'arrhythmia-reminder',
                'Cardiomyopathy': 'cardiomyopathy-reminder',
                'LV Dysfunction': 'lv-dysfunction-reminder',
                'Syncope': 'syncope-reminder',
                'Pre-operative Evaluation': 'pre-op-reminder',
                'Other': 'other-reminder'
            };

            const reminderId = reminderMap[indication];
            if (reminderId) {
                document.getElementById(reminderId).style.display = 'block';
            }
        }

        function hideReminder(indication) {
            const reminderMap = {
                'Valvular Disease': 'valvular-reminder',
                'Cardiac arrhythmia': 'arrhythmia-reminder',
                'Cardiomyopathy': 'cardiomyopathy-reminder',
                'LV Dysfunction': 'lv-dysfunction-reminder',
                'Syncope': 'syncope-reminder',
                'Pre-operative Evaluation': 'pre-op-reminder',
                'Other': 'other-reminder'
            };

            const reminderId = reminderMap[indication];
            if (reminderId) {
                document.getElementById(reminderId).style.display = 'none';
            }
        }

        function validateSelections() {
            const errors = [];
            const indications = Array.from(selectedIndications);

            // Cannot select ACS <= 24 hrs with ACS > 24 hrs
            if (indications.includes('ACS <= 24 hrs') && indications.includes('ACS > 24 hrs')) {
                errors.push('Cannot select both "ACS ≤ 24 hrs" and "ACS > 24 hrs"');
            }

            // Cannot select New Onset Angina with Worsening Angina
            if (indications.includes('New Onset Angina <= 2 months') && indications.includes('Worsening Angina')) {
                errors.push('Cannot select both "New Onset Angina ≤ 2 months" and "Worsening Angina"');
            }

            // Stable Known CAD restrictions
            if (indications.includes('Stable Known CAD')) {
                const incompatible = ['ACS <= 24 hrs', 'ACS > 24 hrs', 'New Onset Angina <= 2 months', 
                                     'Worsening Angina', 'Suspected CAD', 'Resuscitated Cardiac Arrest'];
                const conflicts = incompatible.filter(ind => indications.includes(ind));
                if (conflicts.length > 0) {
                    errors.push(`Cannot select "Stable Known CAD" with: ${conflicts.join(', ')}`);
                }
            }

            displayErrors(errors);
            return errors.length === 0;
        }

        function displayErrors(errors) {
            const errorDiv = document.getElementById('validation-errors');
            if (errors.length === 0) {
                errorDiv.innerHTML = '';
                return;
            }

            let html = '';
            errors.forEach(error => {
                html += `<div class="validation-error">⚠️ ${error}</div>`;
            });
            errorDiv.innerHTML = html;
        }

        function updateSummary() {
            const listElement = document.getElementById('selected-indications');
            
            if (selectedIndications.size === 0) {
                listElement.innerHTML = '<li class="no-selection">No indications selected yet</li>';
            } else {
                // Sort by priority order
                const sorted = Array.from(selectedIndications).sort((a, b) => {
                    return priorityOrder.indexOf(a) - priorityOrder.indexOf(b);
                });

                listElement.innerHTML = sorted.map(ind => `<li>${ind}</li>`).join('');
            }

            determinePCIIndication();
        }

        function determinePCIIndication() {
            if (selectedIndications.size === 0) {
                document.getElementById('pci-indication').innerHTML = 
                    '<span class="no-selection">Will be determined based on Cath Lab Indications</span>';
                document.getElementById('pci-indication-section').classList.add('hidden');
                return;
            }

            if (!validateSelections()) {
                document.getElementById('pci-indication').innerHTML = 
                    '<span class="no-selection">Please resolve validation errors first</span>';
                return;
            }

            // Get highest priority indication
            const sorted = Array.from(selectedIndications).sort((a, b) => {
                return priorityOrder.indexOf(a) - priorityOrder.indexOf(b);
            });
            const highest = sorted[0];

            // Determine PCI Indication based on highest Cath Lab Indication
            if (highest === 'ACS <= 24 hrs') {
                showPCISubQuestion('stemi-acs24');
            } else if (highest === 'ACS > 24 hrs') {
                showPCISubQuestion('stemi-acs24plus');
            } else if (highest === 'New Onset Angina <= 2 months') {
                setPCIIndication('New Onset Angina <= 2 months');
            } else if (highest === 'Stable Known CAD') {
                showPCISubQuestion('stable-angina');
            } else {
                // All other indications map to "Other PCI Indication"
                setPCIIndication('Other PCI Indication');
            }
        }

        function showPCISubQuestion(questionType) {
            const section = document.getElementById('pci-indication-section');
            section.classList.remove('hidden');

            if (questionType === 'stemi-acs24') {
                section.innerHTML = `
                    <div class="pci-sub-question">
                        <div class="pci-sub-question-title">Additional Information Needed for PCI Indication:</div>
                        <p style="margin-bottom: 15px; font-weight: 600;">Did the patient present with a STEMI?</p>
                        <div class="option" onclick="handlePCISubAnswer('stemi-acs24', 'yes')">
                            <input type="radio" name="pci-sub" id="pci-stemi-yes-24" value="yes">
                            <label for="pci-stemi-yes-24">Yes - Consider timing:</label>
                        </div>
                        <div class="sub-option-group" id="pci-stemi-timing-24" style="display: none;">
                            <div class="option" onclick="selectPCIOption('STEMI - Immediate PCI for Acute STEMI')">
                                <input type="radio" name="stemi-timing-24" id="stemi-immediate" value="STEMI - Immediate PCI for Acute STEMI">
                                <label for="stemi-immediate">STEMI - Immediate PCI for Acute STEMI</label>
                            </div>
                            <div class="option" onclick="selectPCIOption('STEMI - Stable (<= 12 hrs from Sx)')">
                                <input type="radio" name="stemi-timing-24" id="stemi-stable-12" value="STEMI - Stable (<= 12 hrs from Sx)">
                                <label for="stemi-stable-12">STEMI - Stable (≤ 12 hrs from Sx)</label>
                            </div>
                            <div class="option" onclick="selectPCIOption('STEMI - Stable (> 12 hrs from Sx)')">
                                <input type="radio" name="stemi-timing-24" id="stemi-stable-12plus" value="STEMI - Stable (> 12 hrs from Sx)">
                                <label for="stemi-stable-12plus">STEMI - Stable (> 12 hrs from Sx)</label>
                            </div>
                            <div class="option" onclick="selectPCIOption('STEMI - Unstable (> 12 hrs from Sx)')">
                                <input type="radio" name="stemi-timing-24" id="stemi-unstable" value="STEMI - Unstable (> 12 hrs from Sx)">
                                <label for="stemi-unstable">STEMI - Unstable (> 12 hrs from Sx)</label>
                            </div>
                            <div class="option" onclick="selectPCIOption('STEMI (after successful lytics)')">
                                <input type="radio" name="stemi-timing-24" id="stemi-success-lytic" value="STEMI (after successful lytics)">
                                <label for="stemi-success-lytic">STEMI (after successful lytics)</label>
                            </div>
                            <div class="option" onclick="selectPCIOption('STEMI - Rescue (After unsuccessful lytics)')">
                                <input type="radio" name="stemi-timing-24" id="stemi-rescue" value="STEMI - Rescue (After unsuccessful lytics)">
                                <label for="stemi-rescue">STEMI - Rescue (After unsuccessful lytics)</label>
                            </div>
                        </div>
                        <div class="option" onclick="handlePCISubAnswer('stemi-acs24', 'no')">
                            <input type="radio" name="pci-sub" id="pci-stemi-no-24" value="no">
                            <label for="pci-stemi-no-24">No - NSTE-ACS</label>
                        </div>
                    </div>
                `;
            } else if (questionType === 'stemi-acs24plus') {
                section.innerHTML = `
                    <div class="pci-sub-question">
                        <div class="pci-sub-question-title">Additional Information Needed for PCI Indication:</div>
                        <p style="margin-bottom: 15px; font-weight: 600;">Did the patient present with a STEMI?</p>
                        <div class="option" onclick="handlePCISubAnswer('stemi-acs24plus', 'yes')">
                            <input type="radio" name="pci-sub" id="pci-stemi-yes-24plus" value="yes">
                            <label for="pci-stemi-yes-24plus">Yes - Consider timing:</label>
                        </div>
                        <div class="sub-option-group" id="pci-stemi-timing-24plus" style="display: none;">
                            <div class="option" onclick="selectPCIOption('STEMI - Stable (> 12 hrs from Sx)')">
                                <input type="radio" name="stemi-timing-24plus" id="stemi-stable-12plus-2" value="STEMI - Stable (> 12 hrs from Sx)">
                                <label for="stemi-stable-12plus-2">STEMI - Stable (> 12 hrs from Sx)</label>
                            </div>
                            <div class="option" onclick="selectPCIOption('STEMI - Unstable (> 12 hrs from Sx)')">
                                <input type="radio" name="stemi-timing-24plus" id="stemi-unstable-2" value="STEMI - Unstable (> 12 hrs from Sx)">
                                <label for="stemi-unstable-2">STEMI - Unstable (> 12 hrs from Sx)</label>
                            </div>
                            <div class="option" onclick="selectPCIOption('STEMI (after successful lytics)')">
                                <input type="radio" name="stemi-timing-24plus" id="stemi-success-lytic-2" value="STEMI (after successful lytics)">
                                <label for="stemi-success-lytic-2">STEMI (after successful lytics)</label>
                            </div>
                            <div class="option" onclick="selectPCIOption('STEMI - Rescue (After unsuccessful lytics)')">
                                <input type="radio" name="stemi-timing-24plus" id="stemi-rescue-2" value="STEMI - Rescue (After unsuccessful lytics)">
                                <label for="stemi-rescue-2">STEMI - Rescue (After unsuccessful lytics)</label>
                            </div>
                        </div>
                        <div class="option" onclick="handlePCISubAnswer('stemi-acs24plus', 'no')">
                            <input type="radio" name="pci-sub" id="pci-stemi-no-24plus" value="no">
                            <label for="pci-stemi-no-24plus">No - NSTE-ACS</label>
                        </div>
                    </div>
                `;
            } else if (questionType === 'stable-angina') {
                section.innerHTML = `
                    <div class="pci-sub-question">
                        <div class="pci-sub-question-title">Additional Information Needed for PCI Indication:</div>
                        <p style="margin-bottom: 15px; font-weight: 600;">Is the patient experiencing angina/anginal equivalent symptoms?</p>
                        <div class="option" onclick="handlePCISubAnswer('stable-angina', 'yes')">
                            <input type="radio" name="pci-sub" id="pci-angina-yes" value="yes">
                            <label for="pci-angina-yes">Yes - Stable Angina</label>
                        </div>
                        <div class="option" onclick="handlePCISubAnswer('stable-angina', 'no')">
                            <input type="radio" name="pci-sub" id="pci-angina-no" value="no">
                            <label for="pci-angina-no">No - CAD (without ischemic Sx)</label>
                        </div>
                    </div>
                `;
            }
        }

        function handlePCISubAnswer(questionType, answer) {
            pciSubAnswers[questionType] = answer;

            if (questionType === 'stemi-acs24') {
                if (answer === 'yes') {
                    document.getElementById('pci-stemi-timing-24').style.display = 'block';
                } else {
                    setPCIIndication('NSTE-ACS');
                }
            } else if (questionType === 'stemi-acs24plus') {
                if (answer === 'yes') {
                    document.getElementById('pci-stemi-timing-24plus').style.display = 'block';
                } else {
                    setPCIIndication('NSTE-ACS');
                }
            } else if (questionType === 'stable-angina') {
                if (answer === 'yes') {
                    setPCIIndication('Stable angina');
                } else {
                    setPCIIndication('CAD (without ischemic Sx)');
                }
            }
        }

        function selectPCIOption(indication) {
            setPCIIndication(indication);
        }

        function setPCIIndication(indication) {
            document.getElementById('pci-indication').innerHTML = 
                `<div class="pci-indication-value">${indication}</div>`;
        }

        function resetForm() {
            // Clear all selections
            selectedIndications.clear();
            pciSubAnswers = {};

            // Reset all radio buttons
            document.querySelectorAll('input[type="radio"]').forEach(radio => {
                radio.checked = false;
            });

            // Reset all checkboxes
            document.querySelectorAll('input[type="checkbox"]').forEach(checkbox => {
                checkbox.checked = false;
            });

            // Hide all sub-groups
            document.getElementById('acs-timing-group').style.display = 'none';
            document.getElementById('angina-history-group').style.display = 'none';
            document.getElementById('cad-stability-group').style.display = 'none';
            document.getElementById('suspected-cad-group').style.display = 'none';

            // Hide all reminders
            document.querySelectorAll('.reminder').forEach(reminder => {
                reminder.style.display = 'none';
            });

            // Clear validation errors
            document.getElementById('validation-errors').innerHTML = '';

            // Reset summary
            updateSummary();
        }
    </script>
</body>
</html>
