# Dd
Sa
<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ระบบบัตรคิวออนไลน์ (1 - 1000)</title>
    <!-- Tailwind CSS สำหรับจัดหน้าตาให้สวยงาม -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Sarabun:wght@300;400;600;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Sarabun', sans-serif; }
    </style>
</head>
<body class="bg-slate-100 min-h-screen flex flex-col justify-between">

    <!-- Header -->
    <header class="bg-blue-600 text-white shadow-md py-4 px-6 text-center">
        <h1 class="text-2xl font-bold">ระบบจองและเรียกคิวออนไลน์</h1>
        <p class="text-sm text-blue-100">ช่วงคิวบริการ: 1 - 1000</p>
    </header>

    <!-- Main Content -->
    <main class="container mx-auto px-4 py-8 max-w-4xl grid grid-cols-1 md:grid-cols-2 gap-6">

        <!-- ส่วนที่ 1: สำหรับผู้รับบริการ (กดรับคิว) -->
        <section class="bg-white rounded-2xl shadow-lg p-6 flex flex-col items-center justify-between text-center border-t-4 border-blue-500">
            <div>
                <h2 class="text-xl font-semibold text-gray-700 mb-2">บัตรคิวของคุณ</h2>
                <p class="text-gray-500 text-sm mb-6">กดปุ่มด้านล่างเพื่อรับหมายเลขคิวสำหรับเข้ารับบริการ</p>
                
                <div id="myTicketBox" class="hidden">
                    <span class="text-xs uppercase tracking-wider text-gray-400 block">หมายเลขคิวของคุณ</span>
                    <div id="myTicketNum" class="text-6xl font-bold text-blue-600 my-4">-</div>
                </div>
                <div id="noTicketBox" class="py-8">
                    <p class="text-gray-400 italic">ยังไม่มีคิวในระบบของคุณ</p>
                </div>
            </div>

            <div class="w-full mt-6">
                <button onclick="getNewTicket()" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-6 rounded-xl shadow transition duration-200 text-lg">
                    รับบัตรคิว
                </button>
            </div>
        </section>

        <!-- ส่วนที่ 2: แสดงสถานะคิวปัจจุบัน (สำหรับจอมอนิเตอร์) -->
        <section class="bg-white rounded-2xl shadow-lg p-6 flex flex-col items-center justify-between text-center border-t-4 border-emerald-500">
            <div class="w-full">
                <h2 class="text-xl font-semibold text-gray-700 mb-4">สถานะคิวปัจจุบัน</h2>
                
                <div class="bg-emerald-50 border border-emerald-200 rounded-xl p-6 mb-4">
                    <span class="text-xs uppercase tracking-wider text-emerald-600 font-bold block mb-1">กำลังเรียกคิวที่</span>
                    <div id="currentQueueNum" class="text-7xl font-extrabold text-emerald-700">0</div>
                </div>

                <div class="flex justify-around text-gray-600 text-sm">
                    <div>คิวล่าสุดในระบบ: <span id="latestQueueNum" class="font-bold text-gray-800">0</span></div>
                    <div>คิวรอรับบริการ: <span id="waitingCount" class="font-bold text-gray-800">0</span></div>
                </div>
            </div>

            <!-- ส่วนควบคุมสำหรับเจ้าหน้าที่ -->
            <div class="w-full mt-6 border-t pt-4">
                <p class="text-xs text-gray-400 mb-2">ส่วนควบคุมสำหรับเจ้าหน้าที่</p>
                <div class="flex gap-2">
                    <button onclick="nextQueue()" class="flex-1 bg-emerald-600 hover:bg-emerald-700 text-white font-bold py-2.5 px-4 rounded-xl shadow transition duration-200">
                        เรียกคิวถัดไป ➡
                    </button>
                    <button onclick="resetSystem()" class="bg-red-100 hover:bg-red-200 text-red-600 text-sm font-semibold py-2.5 px-4 rounded-xl transition duration-200">
                        รีเซ็ตระบบ
                    </button>
                </div>
            </div>
        </section>

    </main>

    <!-- Footer -->
    <footer class="bg-white text-center py-4 text-xs text-gray-500 border-t">
        Queue Management System &copy; 2026
    </footer>

    <!-- JavaScript สำหรับจัดการระบบคิว -->
    <script>
        // กำหนดค่าเริ่มต้นจาก localStorage หรือค่าเริ่มต้นระบบ
        let currentQueue = parseInt(localStorage.getItem('currentQueue')) || 0;
        let latestQueue = parseInt(localStorage.getItem('latestQueue')) || 0;
        let myTicket = localStorage.getItem('myTicket') || null;

        const MAX_QUEUE = 1000;

        // อัปเดตการแสดงผลเมื่อโหลดหน้าเว็บ
        window.onload = function() {
            updateDisplay();
            if (myTicket) {
                showMyTicket(myTicket);
            }
        };

        // ฟังก์ชันกดรับคิวใหม่
        function getNewTicket() {
            if (latestQueue >= MAX_QUEUE) {
                alert('คิวเต็มจำนวนสูงสุด (1,000 คิว) แล้ววันนี้');
                return;
            }

            latestQueue++;
            myTicket = latestQueue;

            // บันทึกลง localStorage
            localStorage.setItem('latestQueue', latestQueue);
            localStorage.setItem('myTicket', myTicket);

            showMyTicket(myTicket);
            updateDisplay();
        }

        // ฟังก์ชันเรียกคิวถัดไป (เจ้าหน้าที่)
        function nextQueue() {
            if (currentQueue >= latestQueue) {
                alert('ไม่มีคิวรอรับบริการในขณะนี้');
                return;
            }

            currentQueue++;
            localStorage.setItem('currentQueue', currentQueue);
            updateDisplay();
        }

        // ฟังก์ชันรีเซ็ตระบบ
        function resetSystem() {
            if (confirm('คุณต้องการรีเซ็ตระบบคิวทั้งหมดใช่หรือไม่?')) {
                currentQueue = 0;
                latestQueue = 0;
                myTicket = null;

                localStorage.clear();
                updateDisplay();

                document.getElementById('myTicketBox').classList.add('hidden');
                document.getElementById('noTicketBox').classList.remove('hidden');
                alert('รีเซ็ตระบบเรียบร้อยแล้ว');
            }
        }

        // แสดงผลคิวของผู้ใช้
        function showMyTicket(ticketNum) {
            document.getElementById('myTicketNuminnerText') = ticketNum;
            document.getElementById('myTicketNum').innerText = ticketNum;
            document.getElementById('myTicketBox').classList.remove('hidden');
            document.getElementById('noTicketBox').classList.add('hidden');
        }

        // อัปเดตตัวเลขหน้าจอทั้งหมด
        function updateDisplay() {
            document.getElementById('currentQueueNum').innerText = currentQueue;
            document.getElementById('latestQueueNum').innerText = latestQueue;
            
            let waiting = latestQueue - currentQueue;
            document.getElementById('waitingCount').innerText = waiting < 0 ? 0 : waiting;
        }

        // ซิงค์ข้อมูลระหว่างแท็บอัตโนมัติ (ถ้าเปิดหน้าจอผู้ใช้และหน้าจอเจ้าหน้าที่แยกกัน)
        window.addEventListener('storage', function(e) {
            currentQueue = parseInt(localStorage.getItem('currentQueue')) || 0;
            latestQueue = parseInt(localStorage.getItem('latestQueue')) || 0;
            updateDisplay();
        });
    </script>
</body>
</html>
