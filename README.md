# ScriptTradingViewForForex
# ⚡ Scalping EMA Strategy (Pine Script v5)

สคริปต์กลยุทธ์เทรดสั้น (Scalping) สำหรับ TradingView ที่ออกแบบมาให้ตีจังหวะเข้าทำอย่างรวดเร็วใน Timeframe ระดับนาที (เช่น 1m, 3m, 5m)  
โดยอิงจากการผสมผสานของ EMA, RSI, Volume, Price Action และ Higher Timeframe Trend Filter  
เหมาะกับสายเทรดเดย์ เทรดเร็ว ลั่นไว สายมือไวใจถึง

---

## 🧠 กลยุทธ์ที่ใช้

### ✅ Entry Logic (Long/Short)
- **Strong Scalp Buy/Sell**: เมื่อมีสัญญาณ crossover ของ EMA พร้อมแรง RSI และ candle ที่แสดง momentum พร้อม volume สูง
- **Quick Scalp**: ถ้า Strong ยังไม่มา จะใช้สัญญาณเบื้องต้นจาก EMA crossover + RSI ที่อยู่ในโซนพอดี
- **Momentum Buy/Sell**: ตรวจสอบจังหวะเทรนด์ต่อเนื่องสำหรับคนอยากถือซ้อนเทรนด์

### 🛑 Stop Loss / 🤑 Take Profit
- ใช้ค่า ATR (Average True Range) เพื่อวาง SL/TP ที่เหมาะกับความผันผวน ณ ขณะนั้น
- มี multiplier สำหรับปรับ SL/TP ตามความชอบส่วนตัว

### 📈 ตัวช่วยยืนยันแนวโน้ม
- ใช้ Higher Timeframe (HTF) EMA เพื่อกรองเทรนด์ใหญ่ เช่น ใช้กราฟ 15 นาทีเพื่อดูแนวโน้มของกราฟ 1 นาที
- RSI กับ Volume ใช้คัดจังหวะที่แรงจริง ไม่เอาจังหวะปลอม

---

## ⚙️ การปรับ Timeframe ให้เหมาะกับตลาด

ในสคริปต์นี้รองรับการเทรดหลาย Timeframe แต่ถ้าอยากให้แม่นสุดแนะนำดังนี้:

| Timeframe (TF) | HTF ที่แนะนำ | เหมาะกับตลาดแบบไหน            |
|----------------|--------------|----------------------------------|
| 1m             | 5m, 15m      | เหมาะกับ scalping เร็วสุดๆ       |
| 3m             | 15m, 30m     | จังหวะไว แต่กรอง noise ได้มากขึ้น |
| 5m             | 15m, 1h      | ความแม่นสูงขึ้น เทรดน้อยลง       |

> **จะเปลี่ยน Timeframe ยังไง?**
1. เปลี่ยน TF ในชาร์ต (1m, 3m, ฯลฯ)
2. ปรับ `htfTimeframe` ที่ input ของสคริปต์ เช่น จาก `"15"` เป็น `"30"` หรือ `"1H"`

---

## 🧪 ตัวแปรและการปรับแต่ง

```pine
shortEmaLen      // EMA เร็ว (ค่า default: 5)
longEmaLen       // EMA ช้า (ค่า default: 13)
rsiLen           // ความยาว RSI (ค่า default: 9)
rsiOverbought    // โซน overbought RSI (default: 75)
rsiOversold      // โซน oversold RSI (default: 25)
atrPeriod        // คำนวณ ATR (default: 10)
atrMultSL        // คูณ ATR สำหรับ Stop Loss (default: 1.5)
atrMultTP        // คูณ ATR สำหรับ Take Profit (default: 2.0)
volumeMultiplier // เทียบ Volume กับค่าเฉลี่ย (default: 1.3x)


## ⏰ วิธีปรับแต่งสคริปต์ให้เหมาะกับแต่ละช่วงเวลาเทรด Forex

สคริปต์นี้ออกแบบมาให้เทรดสั้น (Scalping) แต่เราสามารถปรับแต่งให้เหมาะกับช่วงเวลาต่างๆ ของการเทรด Forex ได้ง่ายๆ โดยปรับ Timeframe หลักและ Timeframe ที่ใช้กรองแนวโน้ม (Higher Timeframe Filter)

### 1. ปรับ Timeframe หลัก (Chart Timeframe)

- เลือก Timeframe ที่ต้องการเทรดบนกราฟ TradingView เช่น  
  - 1 นาที (1m) สำหรับเทรดเร็วสุด  
  - 5 นาที (5m) สำหรับจังหวะกลางๆ  
  - 1 ชั่วโมง (1H) สำหรับเทรดแบบสวิงระยะสั้น  
  - 1 วัน (1D) สำหรับเทรดแบบสวิงหรือถือยาว

### 2. ปรับตัวกรองแนวโน้ม Timeframe สูง (Higher Timeframe)

- ในสคริปต์จะมีตัวแปรชื่อ `htfTimeframe` ซึ่งเป็น Timeframe ที่ใช้ดึงข้อมูล EMA และเทรนด์หลัก เพื่อช่วยกรองสัญญาณ  
- เปลี่ยนค่าตัวแปรนี้ให้เหมาะกับ Timeframe หลักที่เลือก เช่น  

| Timeframe หลัก | ค่าที่แนะนำสำหรับ `htfTimeframe` |
|----------------|----------------------------------|
| 1m             | 5m หรือ 15m                      |
| 5m             | 15m หรือ 1H                      |
| 1H             | 4H หรือ 1D                      |
| 1D             | 1W หรือ 1M                      |

ตัวอย่างโค้ดที่ปรับได้:

```pinescript
htfTimeframe = input.timeframe("15", title="Higher Timeframe for Trend")  // เปลี่ยนเป็น "60" หรือ "D" ตามต้องการ
