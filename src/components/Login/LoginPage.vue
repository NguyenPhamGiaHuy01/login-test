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
                <!-- Step 1: Employee ID Input -->
                <div v-show="currentStep === 1" class="form-section active">
                    <div class="section-title">Đăng nhập hệ thống</div>
                    <div class="section-subtitle">Vui lòng nhập mã nhân viên để tiếp tục</div>

                    <input type="text" id="employee-id" v-model="employeeId" @input="validateField('employeeId')"
                        class="form-input" placeholder="Mã nhân viên" />
                    <div v-if="errorMessage.employeeId" class="error-message">
                        {{ errorMessage.employeeId }}
                    </div>

                    <input type="password" id="employee-password" v-model="password" @input="validateField('password')"
                        class="form-input" placeholder="Mật khẩu" />
                    <div v-if="errorMessage.password" class="error-message">
                        {{ errorMessage.password }}
                    </div>

                    <button type="button" class="login-btn" @click="loginEmployeeId">
                        <div class="loading"></div>
                        <span class="btn-text">Tiếp tục</span>
                    </button>
                </div>

                <!-- Step 2: Account Not Found -->
                <div v-show="currentStep === 2" class="form-section">
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

                    <button type="button" class="login-btn btn-secondary" @click="goBackToStart">
                        Quay lại
                    </button>
                </div>

                <div v-show="currentStep === 3" class="form-section">
                    <div class="section-title">Xác thực OTP</div>
                    <div class="section-subtitle">Mã OTP đã được gửi đến email của bạn</div>

                    <div class="user-info-card" v-if="currentEmployee">

                        <div class="user-info-title">Thông tin tài khoản</div>
                        <div class="user-info-item">
                            <span class="user-info-label">Mã nhân viên:</span>
                            <span class="user-info-value" id="display-employee-id"> {{ currentEmployee.id }}</span>
                        </div>
                        <div class="user-info-item">
                            <span class="user-info-label">Email:</span>
                            <span class="user-info-value" id="display-email"> {{ currentEmployee.email }}</span>
                        </div>
                    </div>

                    <div class="success-message">
                        <strong>Mã OTP đã được gửi!</strong><br>
                        Vui lòng kiểm tra email và nhập mã xác thực bên dưới
                    </div>

                    <div class="otp-input">
                        <input v-for="(digit, index) in otp" :key="index" type="text" class="otp-digit" maxlength="1"
                            v-model="otp[index]" @input="moveToNext(index)"
                            @keydown.backspace="moveToPrev(index, $event)" ref="otpInputs" />
                    </div>


                    <div id="otp-error" class="error-message" style="display: none; text-align: center;"></div>

                    <div class="resend-section">
                        <div class="resend-text">Không nhận được mã OTP?</div>
                        <a href="#" id="resend-link" class="resend-link" onclick="resendOTP()">Gửi lại mã</a>
                        <span id="resend-countdown" class="countdown" style="display: none;"></span>
                    </div>

                    <button type="button" class="login-btn" @click="verifyOTP" style="margin-top: 20px;">
                        <div class="loading"></div>
                        <span class="btn-text">Xác thực</span>
                    </button>

                    <button type="button" class="login-btn btn-secondary" @click="goBackToStart">
                        Quay lại
                    </button>
                </div>


            </div>
        </div>
    </div>


</template>

<script>
export default {
    name: 'LoginPage',

    data() {
        return {
            employeeDatabase: {
                'EMP001': {
                    id: 'EMP001',
                    name: 'Nguyễn Văn A',
                    email: 'nguyenvana@heineken.com',
                    password: 'Huy1412!'
                },
                'EMP002': {
                    id: 'EMP002',
                    name: 'Trần Thị B',
                    email: 'tranthib@heineken.com',
                    password: 'Huy1412!'
                },
                'EMP003': {
                    id: 'EMP003',
                    name: 'Lê Văn C',
                    email: '',
                    password: 'Huy1412!'
                },
                'EMP004': {
                    id: 'EMP004',
                    name: 'Phạm Thị D',
                    email: 'phamthid@heineken.com',
                    password: 'Huy1412!'
                }
            },

            currentEmployee: null,
            currentStep: 1,
            resendCountdown: 0,
            resendTimer: null,
            employeeId: "",
            errorMessage: {
                employeeId: "",
                password: ""
            },
            otp: ["", "", "", ""],
            password: ""

        }
    },
    methods: {

        goBackToStart() {
            this.currentStep = 1;
            this.errorMessage = "";
        },
        validateField(field) {
            if (field === "employeeId") {
                if (!this.employeeId.trim()) {
                    this.errorMessage.employeeId = "❌ Vui lòng nhập mã nhân viên";
                } else {
                    this.errorMessage.employeeId = "";
                }
            }

            if (field === "password") {
                const regex = /^(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&]).{6,}$/;

                if (!this.password.trim()) {
                    this.errorMessage.password = "❌ Vui lòng nhập mật khẩu";
                } else if (!regex.test(this.password)) {
                    this.errorMessage.password =
                        "❌ Mật khẩu phải có ít nhất 6 ký tự, bao gồm 1 chữ hoa, 1 số và 1 ký tự đặc biệt";
                } else {
                    this.errorMessage.password = "";
                }
            }
            if (this.errorMessage.password == "" && this.errorMessage.employeeId == "") {
                return true;
            }
            return false
        },
        loginEmployeeId() {
           
            if (  !this.validateField("employeeId") && !this.validateField("password")) {
                return;
            }
            this.currentEmployee = this.employeeDatabase[this.employeeId];

            if (!this.currentEmployee) {
                this.errorMessage.employeeId = "❌ Không tìm thấy tài khoản này";
                this.currentStep = 2;
            }

            if (this.password !== this.currentEmployee.password) {
                this.errorMessage.password = "❌ Mật khẩu không đúng";
                return;
            }

            this.errorMessage = { employeeId: "", password: "" };

            // Chuyển bước
            this.currentStep = 3;
        },

        moveToNext(index) {
            if (this.otp[index] && index < this.otp.length - 1) {
                this.$refs.otpInputs[index + 1].focus();
            }
        },
        moveToPrev(index, event) {
            if (!this.otp[index] && index > 0 && event.key === "Backspace") {
                this.$refs.otpInputs[index - 1].focus();
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

            setTimeout(() => {
                btn.classList.remove('btn-loading')
                if (list === '1412' && list.length === 4) {

                    alert('Xác thực OTP thành công!')
                    return
                }
                else {
                    alert('Xác thực OTP không thành công!')
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
    margin-bottom: 16px;

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
</style>
