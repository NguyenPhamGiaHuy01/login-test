<template>
    <div class="home-page">
        <div class="home-container">
            <el-card class="box-card">
                <el-row class="flex text-2xl">
                    <el-col :span="12">
                        <i class="pr-2 text-green-700">★</i>
                        <span class="font-bold">HEINEKEN</span>
                    </el-col>
                    <el-col :span="12" class="text-right">
                        <el-button type="danger">
                            Đăng xuất
                        </el-button>
                    </el-col>
                </el-row>

                <el-col class="text-center text-xl font-medium mt-5">
                    Nguyễn Thị B
                </el-col>
                <el-col class="text-center mb-5">
                    Sale Manager - Khu vực miền Nam
                </el-col>
            </el-card>

            <el-row class="mt-3" :gutter="20">
                <el-col class="text-xl text-end" :span="12">
                    <button
                        :class="!checkOption ? 'border-2 bg-green-800 rounded-3xl  w-11/12 text-white' : 'border-2 rounded-3xl w-11/12 text-black'"
                        @click="checkOptionButton(false)">
                        <h1 class="font-bold text-lg">Chưa tiếp cận</h1>
                        <p>5 outlets</p>
                    </button>
                </el-col>
                <el-col class="text-xl text-start " :span="12">
                    <button
                        :class="checkOption ? 'border-2 bg-green-800 rounded-3xl  w-11/12 text-white' : 'border-2 rounded-3xl w-11/12 text-black'"
                        @click="checkOptionButton(true)">
                        <h1 class="font-bold text-lg">Đổi ý</h1>
                        <p>3 outlets</p>
                    </button>
                </el-col>
            </el-row>
            <el-row class="mt-3" :gutter="20">
                <el-col class="flex justify-end" :span="12">
                    <div
                        class="border-2 border-t-4 border-t-green-600 rounded-t-none rounded-b-2xl p-4 w-11/12 text-center space-y-2">
                        <i class="el-icon-close text-xl"></i>
                        <p class="text-3xl font-bold">5</p>
                        <p>TỔNG OUTLETS</p>
                    </div>

                </el-col>

                <el-col class="flex justify-start" :span="12">
                    <div class="border-2 border-t-4 border-t-green-600 rounded-b-2xl p-4 w-11/12 text-center space-y-2">
                        <i class="el-icon-close text-xl"></i>
                        <p class="text-3xl font-bold ">4</p>
                        <p>CHƯA KHẢO SÁT</p>

                    </div>
                </el-col>
            </el-row>

            <el-row class="mt-3" :gutter="20">
                <el-col class="flex justify-end" :span="12">
                    <div class="border-2 border-t-4 border-t-green-600 rounded-b-2xl p-4 w-11/12 text-center space-y-2">
                        <i class="el-icon-close text-xl"></i>
                        <p class="text-3xl font-bold ">1</p>
                        <p>ĐÃ KHẢO SÁT</p>
                    </div>
                </el-col>

                <el-col class="flex justify-start" :span="12">
                    <div class="border-2 border-t-4 border-t-green-600 rounded-b-2xl p-4 w-11/12 text-center space-y-2">
                        <i class="el-icon-close text-xl"></i>
                        <p class="text-3xl font-bold ">20%</p>
                        <p>TỶ LỆ HOÀN THÀNH</p>

                    </div>
                </el-col>
            </el-row>

            <el-row class="mt-3" :gutter="20">
                <el-col :span="24" class="flex justify-center">
                    <div class="border-2 rounded-xl p-6 w-11/12 text-center ">
                        <el-input class="rounded-xl" placeholder="Tìm kiếm outlet chưa tiếp cận..." v-model="input"
                            suffix-icon="el-icon-search"></el-input>
                    </div>
                </el-col>
            </el-row>

            <el-row class="mt-3" :gutter="20">
                <el-col :span="12" class="flex justify-start">
                    <div class="text-2xl font-bold ml-8">
                        <h2>Outlet chưa tiếp cận</h2>
                    </div>


                </el-col>
                <el-col :span="12" class="flex justify-end" style="padding-right: 40px">
                    <el-button type="info" size="medium">
                        Lọc
                    </el-button>

                </el-col>
            </el-row>


            <el-row class="mt-10" :guiter="20">
                <el-col :span="24" class="flex justify-start">
                    <div class=" ml-8">
                        <p>Hiển thị 5 kết quả</p>
                    </div>
                </el-col>
            </el-row>

            <br><br>
        </div>

    </div>
</template>


<script>


export default {
    name: "HomePage",
    data() {
        return {
            checkOption: false
        }
    },
    created() {
        console.log("Home page created");
    },
    watch:
    {

    },
    methods: {
        checkOptionButton(checkButton) {

            if (!checkButton) {
                console.log("Nút Đổi ý được nhấn true", checkButton);
                this.checkOption = false;
            }
            else {
                console.log("Nút Đổi ý được nhấn false", checkButton);
                this.checkOption = true;
            }
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
.home-page {
    display: flex;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
    padding: 10px;
}

.home-container {
    background: white;
    border-radius: 16px;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
    width: 50%;
    max-width: 800px;
    overflow: hidden;
    min-height: 600px;

}

@media (max-width: 480px) {
    .home-container {
        width: 100%;
        border-radius: 0;
        min-height: 100vh;
        /* chiếm hết chiều cao */
    }
}
</style>