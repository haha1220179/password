<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8" />
  <title>ตู็เซฟ</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      padding: 50px;
      background: #f0f0f0;
    }
    #safe {
      width: 200px;
      height: 200px;
      margin: 0 auto 20px;
      background: url('https://cdn-icons-png.flaticon.com/512/2333/2333911.png') no-repeat center center;
      background-size: contain;
    }
    #message {
      margin-bottom: 20px;
      font-size: 18px;
      color: #333;
    }
    #inputCode {
      padding: 10px;
      font-size: 16px;
      width: 180px;
      margin-bottom: 10px;
    }
    #submitBtn {
      padding: 10px 20px;
      font-size: 16px;
    }
    #blockMessage {
      color: red;
      font-weight: bold;
      font-size: 20px;
    }
  </style>
</head>
<body>

  <div id="safe"></div>

  <div id="message">คุณมีโอกาสแค่เพียงสามครั้งเท่านั้น รหัสที่ 5 ตัว 
  คำใบ้ตัวที่ 1  คู่รัก คู่เคียง คู่สม ตัวที่ 2 อ้างว้าง ปล่าวเปลี่ยว โดดเดี่ยว ตัวที่ 3 เริ่มต้ม สูงสุด ลำดับแรก
  ตัวที่ 4 สายรุ้ง สัปดาห์ สวรรค์ ตัวที่ 5 ตั้งครรภ์ พลูโต 
  ขอให้โชคดี</div>

  <input type="password" id="inputCode" placeholder="ใส่รหัสตู็เซฟ" />
  <br />
  <button id="submitBtn">ยืนยัน</button>

  <div id="blockMessage"></div>

  <script>
    const correctCode = "20179";  // กำหนดรหัสตู็เซฟที่ถูกต้อง
    const maxAttempts = 3;
    const attemptsKey = "safeAttempts";
    const blockedKey = "safeBlocked";

    const inputCode = document.getElementById('inputCode');
    const submitBtn = document.getElementById('submitBtn');
    const message = document.getElementById('message');
    const blockMessage = document.getElementById('blockMessage');
    const safeDiv = document.getElementById('safe');

    // ตรวจสอบสถานะบล็อคจาก localStorage
    function checkBlocked() {
      return localStorage.getItem(blockedKey) === "true";
    }

    // บล็อคอุปกรณ์
    function blockDevice() {
      localStorage.setItem(blockedKey, "true");
      inputCode.style.display = "none";
      submitBtn.style.display = "none";
      message.style.display = "none";
      safeDiv.style.filter = "grayscale(100%)";
      blockMessage.textContent = "อุปกรณ์ของคุณถูกบล็อคถาวรเนื่องจากใส่รหัสผิดเกิน 3 ครั้ง";
    }

    // โหลดจำนวนครั้งผิดจาก localStorage
    function getAttempts() {
      return parseInt(localStorage.getItem(attemptsKey)) || 0;
    }

    // บันทึกจำนวนครั้งผิดใน localStorage
    function setAttempts(n) {
      localStorage.setItem(attemptsKey, n);
    }

    // เมื่อโหลดหน้าเว็บ
    window.onload = () => {
      if (checkBlocked()) {
        blockDevice();
      }
    };

    submitBtn.addEventListener('click', () => {
      if (checkBlocked()) {
        blockDevice();
        return;
      }

      const enteredCode = inputCode.value.trim();

      if (enteredCode === correctCode) {
        alert("ยินดีด้วย! คุณใส่รหัสถูกต้องและเปิดตู็เซฟได้แล้ว รางวัลของคุณคือ พี่ชื่อ ยินดี ดีใจ");
        // รีเซ็ตนับครั้งผิด
        setAttempts(0);
        inputCode.value = "";
      } else {
        let attempts = getAttempts();
        attempts++;
        setAttempts(attempts);

        if (attempts >= maxAttempts) {
          blockDevice();
        } else {
          alert(`รหัสผิด! คุณยังมีโอกาสอีก ${maxAttempts - attempts} ครั้ง`);
          inputCode.value = "";
          inputCode.focus();
        }
      }
    });
  </script>

</body>
</html>
