<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DHL Tracking Login</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            background: #ffffff;
            position: relative;
            overflow: hidden;
        }

        /* Background animated elements */
        .background-shape {
            position: absolute;
            border-radius: 50%;
            opacity: 0.05;
            filter: blur(40px);
            display: none;
        }

        .shape1 {
            width: 300px;
            height: 300px;
            background: #ffb800;
            top: -100px;
            left: -100px;
            animation: float 6s ease-in-out infinite;
        }

        .shape2 {
            width: 200px;
            height: 200px;
            background: #ffb800;
            bottom: -50px;
            right: -50px;
            animation: float 8s ease-in-out infinite reverse;
        }

        @keyframes float {
            0%, 100% { transform: translateY(0px); }
            50% { transform: translateY(20px); }
        }

        /* DHL Header - now yellow */
        .dhl-header {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            background: #ffb800;          /* Changed to DHL yellow */
            padding: 20px 30px;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
            z-index: 1000;
            display: flex;
            align-items: center;
        }

        .dhl-logo {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .dhl-logo svg {
            width: 100px;
            height: auto;
        }

        .header-title {
            margin-left: 30px;
            display: flex;
            flex-direction: column;
            justify-content: center;
        }

        .header-title h2 {
            color: #333;
            font-size: 18px;
            font-weight: 700;
            margin: 0;
            padding: 0;
        }

        .header-title p {
            color: #666;
            font-size: 12px;
            margin: 5px 0 0 0;
            padding: 0;
        }

        /* Main login container */
        .login-container {
            width: 100%;
            max-width: 450px;
            padding: 40px;
            background: #ffffff;
            border: 1px solid #e0e0e0;
            border-radius: 4px;
            box-shadow: 0 2px 12px rgba(0, 0, 0, 0.1);
            z-index: 10;
            animation: slideIn 0.6s ease-out;
            margin-top: 100px;
        }

        @keyframes slideIn {
            from { opacity: 0; transform: translateY(30px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .login-header {
            text-align: center;
            margin-bottom: 40px;
        }

        .login-header h1 {
            color: #333;
            font-size: 28px;
            margin-bottom: 10px;
            font-weight: 700;
        }

        .login-header p {
            color: #666;
            font-size: 14px;
            margin-bottom: 5px;
        }

        .login-header .subtitle {
            color: #999;
            font-size: 12px;
        }

        /* Form group */
        .form-group {
            margin-bottom: 25px;
        }

        label {
            display: block;
            color: #333;
            font-size: 14px;
            font-weight: 500;
            margin-bottom: 10px;
        }

        .input-wrapper {
            position: relative;
            display: flex;
            align-items: center;
        }

        .input-icon {
            position: absolute;
            left: 15px;
            color: #999;
            font-size: 18px;
            z-index: 2;
        }

        input {
            width: 100%;
            padding: 15px 15px 15px 45px;
            border: 1px solid #ddd;
            border-radius: 4px;
            background: #fff;
            color: #333;
            font-size: 16px;
            transition: all 0.3s ease;
        }

        input::placeholder {
            color: #bbb;
        }

        input:focus {
            outline: none;
            background: #fafafa;
            border-color: #ffb800;
            box-shadow: 0 0 5px rgba(255, 184, 0, 0.2);
        }

        input.error {
            border-color: #ff6b6b;
            background: rgba(255, 107, 107, 0.05);
        }

        input.error:focus {
            box-shadow: 0 0 5px rgba(255, 107, 107, 0.3);
        }

        .error-message {
            color: #ff6b6b;
            font-size: 12px;
            margin-top: 8px;
            display: none;
        }

        .error-message.show {
            display: block;
        }

        /* Login button */
        .login-btn {
            width: 100%;
            padding: 15px;
            margin-top: 30px;
            border: none;
            border-radius: 12px;
            background: linear-gradient(135deg, #ffb800 0%, #ffa500 100%);
            color: #333;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }

        .login-btn:hover:not(:disabled) {
            transform: translateY(-2px);
            box-shadow: 0 10px 25px rgba(255, 184, 0, 0.4);
        }

        .login-btn:active:not(:disabled) {
            transform: translateY(0);
        }

        .login-btn:disabled {
            opacity: 0.8;
            cursor: not-allowed;
        }

        .spinner {
            display: none;
            width: 20px;
            height: 20px;
            border: 3px solid rgba(51, 51, 51, 0.3);
            border-top: 3px solid #333;
            border-radius: 50%;
            animation: spin 0.8s linear infinite;
            margin-right: 10px;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .btn-content {
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .spinner.show {
            display: inline-block;
        }

        /* Footer */
        .footer-links {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin-top: 25px;
            padding-top: 25px;
            border-top: 1px solid #e0e0e0;
        }

        .footer-links a {
            color: #666;
            text-decoration: none;
            font-size: 12px;
            transition: color 0.3s ease;
        }

        .footer-links a:hover {
            color: #ffb800;
        }

        /* Success animation */
        @keyframes successPulse {
            0% { background: linear-gradient(135deg, #ffb800 0%, #ffa500 100%); }
            50% { background: linear-gradient(135deg, #4caf50 0%, #45a049 100%); }
            100% { background: linear-gradient(135deg, #4caf50 0%, #45a049 100%); }
        }

        .login-btn.success {
            animation: successPulse 0.6s ease-out;
            background: linear-gradient(135deg, #4caf50 0%, #45a049 100%);
        }

        .response-message {
            text-align: center;
            color: #4caf50;
            font-size: 14px;
            margin-top: 15px;
            display: none;
        }

        .response-message.show {
            display: block;
        }
    </style>
</head>
<body>
    <!-- Background shapes -->
    <div class="background-shape shape1"></div>
    <div class="background-shape shape2"></div>

    <!-- DHL Header with yellow background -->
    <div class="dhl-header">
        <div class="dhl-logo">
            <svg width="120" height="40" viewBox="0 0 300 80" xmlns="http://www.w3.org/2000/svg">
                <text x="0" y="60" font-family="Arial Black, 'Helvetica Neue', Helvetica, Arial, sans-serif" 
                      font-size="70" font-weight="900" fill="#D40511" font-style="italic">DHL</text>
            </svg>
        </div>
        <div class="header-title">
            <h2>Track & Trace</h2>
            <p>Shipment Management Portal</p>
        </div>
    </div>

    <!-- Login Container -->
    <div class="login-container">
        <div class="login-header">
            <h1>Enter Tracking Details</h1>
            <p>Access your shipment information</p>
            <div class="subtitle">Please provide your tracking information to continue</div>
        </div>

        <form id="loginForm">
            <!-- Tracking ID Input -->
            <div class="form-group">
                <label for="trackingId">Tracking ID</label>
                <div class="input-wrapper">
                    <span class="input-icon">📦</span>
                    <input 
                        type="text" 
                        id="trackingId" 
                        name="trackingId" 
                        placeholder="Enter your tracking number"
                        autocomplete="off"
                    >
                </div>
                <div class="error-message" id="trackingError">Invalid Tracking ID</div>
            </div>

            <!-- Security Code Input -->
            <div class="form-group">
                <label for="securityCode">Security Code</label>
                <div class="input-wrapper">
                    <span class="input-icon">🔐</span>
                    <input 
                        type="password" 
                        id="securityCode" 
                        name="securityCode" 
                        placeholder="Enter security code"
                        autocomplete="off"
                    >
                </div>
                <div class="error-message" id="securityError">Invalid Security Code</div>
            </div>

            <!-- Login Button -->
            <button type="submit" class="login-btn" id="loginBtn">
                <div class="btn-content">
                    <span class="spinner" id="spinner"></span>
                    <span id="btnText">Access Shipment</span>
                </div>
            </button>

            <!-- Response Message -->
            <div class="response-message" id="responseMsg">
                Authentication successful! Redirecting...
            </div>
        </form>

        <!-- Footer Links -->
        <div class="footer-links">
            <a href="#forgot">Forgot Tracking ID?</a>
            <a href="#support">Support</a>
            <a href="#help">Need Help?</a>
        </div>
    </div>

    <script>
        // Correct credentials (for demo purposes only)
        const CORRECT_TRACKING_ID = 'KNX54839201';
        const CORRECT_SECURITY_CODE = 'DM7698214';

        // Get elements
        const form = document.getElementById('loginForm');
        const trackingIdInput = document.getElementById('trackingId');
        const securityCodeInput = document.getElementById('securityCode');
        const trackingError = document.getElementById('trackingError');
        const securityError = document.getElementById('securityError');
        const loginBtn = document.getElementById('loginBtn');
        const spinner = document.getElementById('spinner');
        const btnText = document.getElementById('btnText');
        const responseMsg = document.getElementById('responseMsg');

        // Remove error message on input
        trackingIdInput.addEventListener('input', function() {
            trackingIdInput.classList.remove('error');
            trackingError.classList.remove('show');
        });

        securityCodeInput.addEventListener('input', function() {
            securityCodeInput.classList.remove('error');
            securityError.classList.remove('show');
        });

        // Form submission
        form.addEventListener('submit', function(e) {
            e.preventDefault();

            const trackingId = trackingIdInput.value.trim().toUpperCase();
            const securityCode = securityCodeInput.value.trim();

            // Reset error states
            trackingIdInput.classList.remove('error');
            securityCodeInput.classList.remove('error');
            trackingError.classList.remove('show');
            securityError.classList.remove('show');
            responseMsg.classList.remove('show');

            let isValid = true;

            if (trackingId !== CORRECT_TRACKING_ID) {
                trackingIdInput.classList.add('error');
                trackingError.classList.add('show');
                isValid = false;
            }

            if (securityCode !== CORRECT_SECURITY_CODE) {
                securityCodeInput.classList.add('error');
                securityError.classList.add('show');
                isValid = false;
            }

            if (isValid) {
                loginBtn.disabled = true;
                spinner.classList.add('show');
                btnText.textContent = 'Verifying...';
                loginBtn.classList.add('success');

                setTimeout(() => {
                    responseMsg.classList.add('show');
                    btnText.textContent = 'Redirecting...';
                }, 500);

                setTimeout(() => {
                    window.location.href = 'tracking.html';
                }, 2500);
            }
        });

        // Allow Enter key submission
        trackingIdInput.addEventListener('keypress', function(e) {
            if (e.key === 'Enter') form.dispatchEvent(new Event('submit'));
        });

        securityCodeInput.addEventListener('keypress', function(e) {
            if (e.key === 'Enter') form.dispatchEvent(new Event('submit'));
        });
    </script>
</body>
</html>
