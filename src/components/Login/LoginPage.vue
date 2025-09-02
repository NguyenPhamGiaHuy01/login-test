<template>
    <!-- LOGIN PAGE -->
    <div id="login-page" class="login-page">
        <div class="login-container">
            <div class="header">
                <div class="logo">
                    <div class="star">★</div>
                    <div class="logo-text">HEINEKEN</div>
                    <div class="subtitle">Sales Portal</div>
                </div>
            </div>

            <div class="content">

                <div class="step-indicator">
                    <div class="step-dot active" id="step-1"></div>
                    <div class="step-dot" id="step-2"></div>
                    <div class="step-dot" id="step-3"></div>
                    <div class="step-dot" id="step-4"></div>
                </div>

                <!-- Step 1: Employee ID Input -->
                <div id="step-employee-id" class="form-section active">
                    <div class="section-title">Đăng nhập hệ thống</div>
                    <div class="section-subtitle">Vui lòng nhập mã nhân viên để tiếp tục</div>

                    <div class="form-group">
                        <label class="form-label">Mã nhân viên</label>
                        <input type="text" id="employee-id" class="form-input" placeholder="Nhập mã nhân viên"
                            required="">
                        <div id="employee-id-error" class="error-message" style="display: none;"></div>
                    </div>

                    <button type="button" class="login-btn" onclick="checkEmployeeId()">
                        <div class="loading"></div>
                        <span class="btn-text">Tiếp tục</span>
                    </button>
                </div>

                <!-- Step 2: Account Not Found -->
                <!-- <div id="step-not-found" class="form-section">
                    <div class="section-title">Tài khoản chưa được đăng ký</div>
                    <div class="section-subtitle">Hệ thống không tìm thấy thông tin tài khoản của bạn</div>

                    <div class="warning-message">
                        <strong>Tài khoản chưa được đăng ký!</strong><br>
                        Vui lòng liên hệ IT để được hỗ trợ tạo tài khoản.
                    </div>

                    <div class="contact-info">
                        <div class="contact-title">Thông tin liên hệ IT Support</div>
                        <div class="contact-details">
                            📧 Email: help@twin.vn<br>
                            📞 Hotline: 1900-xxxx<br>
                            ⏰ Thời gian hỗ trợ: 8:00 - 17:30
                        </div>
                    </div>

                    <button type="button" class="login-btn btn-secondary" onclick="goBackToStart()">
                        Quay lại
                    </button>
                </div> -->
               
            </div>
        </div>
    </div>

    <!-- DASHBOARD -->
    <!-- <div id="dashboard" class="dashboard">
        <div class="dashboard-header">
            <div class="dashboard-title">Chào mừng đến Sales Portal</div>
            <div class="dashboard-subtitle">Đăng nhập thành công!</div>
            <div style="margin-top: 16px; color: #64748b; font-size: 14px;">
                Tài khoản: <span id="logged-user-info"></span>
            </div>
            <button class="logout-btn" onclick="logout()">Đăng xuất</button>
        </div>

    </div> -->
</template>

<script>
export default {
    name: 'LoginPage',


    methods: {
        moveToNext(event, index) {
            const current = event.target;
            if (current.value.length === 1 && index < 3) {
                const nextInput = document.querySelectorAll(".otp-digit")[index + 1];
                nextInput.focus()
            }

        },

        verifyOTP(event) {
            const otpInputs = document.querySelectorAll('.otp-digit');
            const list = Array.from(otpInputs)
                .map(input => input.value)
                .filter(v => v !== "")
                .join('');


            console.log("value", list)
            console.log("value", list.length)
            const errorDiv = document.getElementById('otp-error');
            if (list.length !== 4) {
                errorDiv.textContent = 'Vui lòng nhập đầy đủ mã OTP';
                errorDiv.style.display = 'block';
                return;
            }

            const btn = event.target;
            btn.classList.add('btn-loading');

            // // Simulate OTP verification
            // setTimeout(() => {
            //     btn.classList.remove('btn-loading');

            //     // For demo, accept any 6-digit OTP
            //     if (otp === '123456' || otp.length === 6) {
            //         goToPasswordStep(true);
            //     } else {
            //         errorDiv.textContent = 'Mã OTP không đúng. Vui lòng thử lại.';
            //         errorDiv.style.display = 'block';
            //         // Clear OTP inputs
            //         otpInputs.forEach(input => input.value = '');
            //         otpInputs[0].focus();
            //     }
            // }, 1500);
            setTimeout(() => {
                btn.classList.remove('btn-loading')
                if (list === '1412' && list.length === 4) {
                    //goToPasswordStep(true);
                    console.log("YES")
                    return;
                }
                else {
                    console.log("NO")
                    return;
                }

            }, 1500);
        }

    }
}



</script>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    background: #f8fafc;
    min-height: 100vh;
    color: #1e293b;
}

/* LOGIN STYLES */
.login-page {
    display: flex;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
    padding: 20px;
}

.login-container {
    background: white;
    border-radius: 16px;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
    width: 100%;
    max-width: 400px;
    overflow: hidden;
    min-height: 500px;
}

.header {
    background: linear-gradient(135deg, #00a63f 0%, #066d29 100%);
    padding: 32px 24px;
    text-align: center;
}

.logo .star {
    font-size: 36px;
    color: white;
    margin-bottom: 8px;
}

.logo-text {
    color: white;
    font-size: 24px;
    font-weight: 700;
    margin-bottom: 4px;
}

.subtitle {
    color: rgba(255, 255, 255, 0.9);
    font-size: 14px;
}

.content {
    padding: 32px 24px;
    min-height: 300px;
    display: flex;
    flex-direction: column;
    justify-content: center;
}

.step-indicator {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: 24px;
    gap: 8px;
}

.step-dot {
    width: 8px;
    height: 8px;
    border-radius: 50%;
    background: #e5e7eb;
    transition: all 0.3s ease;
}

.step-dot.active {
    background: #00a63f;
    width: 24px;
    border-radius: 4px;
}

.step-dot.completed {
    background: #00a63f;
}

.form-section {
    /* display: none; */
    animation: slideIn 0.4s ease;
}

.form-section.active {
    display: block;
}

@keyframes slideIn {
    from {
        opacity: 0;
        transform: translateY(10px);
    }

    to {
        opacity: 1;
        transform: translateY(0);
    }
}

.section-title {
    font-size: 20px;
    font-weight: 700;
    color: #1e293b;
    text-align: center;
    margin-bottom: 8px;
}

.section-subtitle {
    font-size: 14px;
    color: #64748b;
    text-align: center;
    margin-bottom: 24px;
    line-height: 1.5;
}

.form-group {
    margin-bottom: 20px;
}

.form-label {
    display: block;
    margin-bottom: 8px;
    color: #374151;
    font-weight: 600;
    font-size: 14px;
}

.form-input {
    width: 100%;
    padding: 14px 16px;
    border: 2px solid #e5e7eb;
    border-radius: 12px;
    font-size: 16px;
    transition: all 0.3s ease;
    background: #fafbfc;
}

.form-input:focus {
    outline: none;
    border-color: #00a63f;
    background: white;
    box-shadow: 0 0 0 3px rgba(0, 166, 63, 0.1);
}

.form-input.error {
    border-color: #ef4444;
    background: #fef2f2;
}

.error-message {
    color: #ef4444;
    font-size: 12px;
    margin-top: 6px;
    font-weight: 500;
}

.success-message {
    background: #dcfce7;
    border: 1px solid #16a34a;
    border-radius: 12px;
    padding: 16px;
    color: #15803d;
    font-weight: 500;
    margin-bottom: 20px;
    text-align: center;
}

.warning-message {
    background: #fef3c7;
    border: 1px solid #f59e0b;
    border-radius: 12px;
    padding: 20px;
    color: #92400e;
    font-weight: 500;
    margin-bottom: 20px;
    text-align: center;
    line-height: 1.5;
}

.info-message {
    background: #dbeafe;
    border: 1px solid #3b82f6;
    border-radius: 12px;
    padding: 16px;
    color: #1d4ed8;
    font-weight: 500;
    margin-bottom: 20px;
    font-size: 14px;
    line-height: 1.5;
}

.login-btn {
    width: 100%;
    padding: 16px;
    background: #00a63f;
    color: white;
    border: none;
    border-radius: 12px;
    font-size: 16px;
    font-weight: 700;
    cursor: pointer;
    transition: all 0.3s ease;
    margin-top: 8px;
    position: relative;
}

.login-btn:hover:not(:disabled) {
    background: #047d31;
}

.login-btn:disabled {
    background: #94a3b8;
    cursor: not-allowed;
}

.btn-secondary {
    background: #6b7280;
    margin-right: 12px;
    width: 100%;
    padding: 12px 20px;
    display: inline-block;
}

.btn-secondary:hover {
    background: #4b5563;
}

.otp-input {
    display: flex;
    justify-content: center;
    gap: 8px;
    margin: 20px 0;
}

.otp-digit {
    width: 48px;
    height: 48px;
    text-align: center;
    border: 2px solid #e5e7eb;
    border-radius: 12px;
    font-size: 20px;
    font-weight: 700;
    transition: all 0.3s ease;
    background: #fafbfc;
}

.otp-digit:focus {
    outline: none;
    border-color: #00a63f;
    background: white;
    box-shadow: 0 0 0 3px rgba(0, 166, 63, 0.1);
}

.resend-section {
    text-align: center;
    margin-top: 16px;
}

.resend-text {
    color: #64748b;
    font-size: 14px;
    margin-bottom: 8px;
}

.resend-link {
    color: #00a63f;
    text-decoration: none;
    font-weight: 600;
    font-size: 14px;
}

.resend-link:hover {
    text-decoration: underline;
}

.resend-link.disabled {
    color: #94a3b8;
    cursor: not-allowed;
    text-decoration: none;
}

.countdown {
    color: #f59e0b;
    font-weight: 600;
}

.contact-info {
    background: #f8fafc;
    border-radius: 12px;
    padding: 16px;
    margin-top: 20px;
    text-align: center;
}

.contact-title {
    font-size: 14px;
    font-weight: 600;
    color: #374151;
    margin-bottom: 8px;
}

.contact-details {
    font-size: 13px;
    color: #64748b;
    line-height: 1.4;
}

/* LOADING */
.loading {
    display: none;
    width: 16px;
    height: 16px;
    border: 2px solid rgba(255, 255, 255, 0.3);
    border-radius: 50%;
    border-top-color: white;
    animation: spin 1s linear infinite;
    margin: 0 auto;
}

.btn-loading .loading {
    display: inline-block;
    margin-right: 8px;
}

.btn-loading .btn-text {
    opacity: 0.8;
}

@keyframes spin {
    to {
        transform: rotate(360deg);
    }
}

/* USER INFO DISPLAY */
.user-info-card {
    background: #f0fdf4;
    border: 1px solid #22c55e;
    border-radius: 12px;
    padding: 16px;
    margin-bottom: 20px;
}

.user-info-title {
    font-size: 14px;
    font-weight: 600;
    color: #15803d;
    margin-bottom: 8px;
}

.user-info-item {
    display: flex;
    justify-content: space-between;
    margin-bottom: 4px;
}

.user-info-label {
    font-size: 13px;
    color: #166534;
    font-weight: 500;
}

.user-info-value {
    font-size: 13px;
    color: #15803d;
    font-weight: 600;
}

/* RESPONSIVE */
@media (max-width: 375px) {
    .login-container {
        margin: 10px;
        max-width: 100%;
    }

    .content {
        padding: 24px 20px;
    }

    .header {
        padding: 24px 20px;
    }

    .otp-digit {
        width: 44px;
        height: 44px;
        font-size: 18px;
    }

    .otp-input {
        gap: 6px;
    }
}

/* DASHBOARD STYLES - Simplified since focus is on login flow */
.dashboard {
    display: none;
    padding: 20px;
    text-align: center;
}

.dashboard.active {
    display: block;
}

.dashboard-header {
    background: white;
    border-radius: 16px;
    padding: 32px;
    margin-bottom: 24px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.06);
}

.dashboard-title {
    font-size: 28px;
    font-weight: 800;
    color: #1e293b;
    margin-bottom: 8px;
}

.dashboard-subtitle {
    color: #64748b;
    font-size: 16px;
}

.logout-btn {
    background: #ef4444;
    color: white;
    border: none;
    padding: 12px 24px;
    border-radius: 8px;
    font-size: 14px;
    font-weight: 600;
    cursor: pointer;
    margin-top: 24px;
}
</style>
