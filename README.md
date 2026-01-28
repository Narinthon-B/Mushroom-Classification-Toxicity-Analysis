ในธรรมชาติมีเห็ดหลากหลายสายพันธุ์ จนมนุษย์ไม่สามารถแยกแยะได้ด้วยตาเปล่าว่าเห็ดชนิดใดปลอดภัย สามารถบริโภคได้ หรือเห็ดชนิดใดมีพิษร้ายแรง โปรเจกต์นี้จึงพัฒนาขึ้น เพื่อใช้การวิเคราะห์ข้อมูลเป็นเครื่องมือในการช่วยตัดสินใจ และลดความเสี่ยงจากการบริโภคเห็ดที่ไม่ปลอดภัย
  
  **วัตถุประสงค์**
  การนํากระบวนการ Data Analytics และแนวคิดด้าน Big Data มาประยุกต์ใช้เพื่อแก้ปัญหาผ่านขั้นตอนดังนี้

 - Data Understanding: ศึกษาลักษณะของเห็ดทั้ง 23 ลักษณะผ่านข้อมูล เช่น รูปทรงของหมวกเห็ด (Cap-shape), กลิ่น (Odor) หรือสีของสปอร์ (Spore-print-color) เป็นต้น
 - Data Cleaning: จักการกับข้อมูลที่มี Missing Values เพื่อให้ได้ชุดข้อมูลที่พร้อมสําหรับการประมวลผล
 - Predictive Modeling: สร้าง Classification Model เพื่อนําไป Predict จากลักษณะของเห็ดว่ากินได้ หรือกินไม่ได้

![Mushroom_Process](https://drive.google.com/file/d/1INoBgq1eKCzqZCLKVQScowghxeg9ltoB/view?usp=drive_link)

**รายละเอียดข้อมูล (Data Overview)**

 - **ที่มา:** ข้อมูล "Exploring the Mysterious World of Mushrooms" จากเว็บไซต์ Kaggle
	(เข้าชมได้ที่: https://www.kaggle.com/datasets/mehmetisik/mushrooms)
 - **ขนาดของข้อมูล:** 8,124 แถว 23 Attributes
 - **รูปแบบข้อมูล:** ข้อมูลถูกจัดในรูปแบบตัวอักษรย่อ ซึ่งช่วยลดขนาดไฟล์ และเพื่อความรวดเร็วในการประมวลผล
 - **ตัวอย่าง Attributes:**
	 - **Class:** เป็น Target จำแนกระหว่างเห็ดที่กินได้ (Edible) และเห็ดพิษ (Poisonous)
	 - **Cap-shape:** รูปทรงของหมวกเห็ด เช่น Bell, Conical, Flat, Sunken
	 - **Odor:** กลิ่นของเห็ด เช่น Almond, Pungent, None ซึ่งเป็นปัจจัยหลักในการตัดสินใจ
	 - **Stalk-root:** ลักษณะโคนก้าน
	 - **Spore-print-color:** สีของสปอร์



**การจัดเตรียมข้อมูล (Data Preparation)**
ขั้นตอนการทํา Data Cleaning

 1. สํารวจข้อมูลที่เกิด Missing Value จากการสํารวจพบว่ามข้อมูลที่มี Missing Value เฉพาะใน Attribute stalk-root เท่านั้น
    
![Mushroom_MissingValue](https://drive.google.com/file/d/1INoBgq1eKCzqZCLKVQScowghxeg9ltoB/view?usp=drive_link)
 3. เลือกใช้ Operator "Filter Examples" โดยตั้งเงื่อนไขเป็น `is not missing` เพื่อตัดแถวที่มีค่าว่างออกจากการวิเคราะห์
    
![Mushroom_FilterExample](https://drive.google.com/file/d/1INoBgq1eKCzqZCLKVQScowghxeg9ltoB/view?usp=drive_link)

> " เพราะข้อมูลเป็นประเภท Categorical การแทนค่าด้วยค่าเฉลี่ยไม่สามารถทําได้ จึงเลือกตัดข้อมูลแทน "
 - หลังจากทํา Data Cleaning แล้ว จะเหลือข้อมูลจํานวน 5,642 แถว และพร้อมที่จะนําไปสร้างโมเดลต่อไป



**Algorithm ที่เลือกใช้**
จากการเปรียบเทียบประสิทธิภาพของโมเดลต่างๆ ได้ตัดสินใจเลือกใช้ Decision Tree เนื่องจาก

 - **เหตุผลที่เลือกใช้**
	 - Decision Tree ให้ผลลัพธ์ออกมาในแบบที่มนุษย์สามารถอ่านและทำความเข้าใจได้ง่าย
	 - เนื่องจากชุดข้อมูลเห็ดเป็นตัวอักษรย่อทั้งหมด โมเดลนี้สามารถประมวลผลได้โดยตรงโดยไม่ต้องแปลงข้อมูลเป็นตัวเลข ซึ่งช่วยลดความเสี่ยงจากการคลาดเคลื่อนของข้อมูล
	 - จากการทดสอบพบว่าโมเดลให้ค่า Performance แม่นยำถึง 100% สำหรับชุดข้อมูลนี้
	 ![Mushroom_Performance](https://drive.google.com/file/d/1INoBgq1eKCzqZCLKVQScowghxeg9ltoB/view?usp=drive_link)
 
 - **การเปรียบเทียบกับโมลเดลอื่น**
	 - **Logistic Regression / LDA:** ไม่ถูกเลือกเนื่องจากถูกออกแบบมาสำหรับข้อมูลตัวเลข และมีความซับซ้อนในการเตรียมข้อมูลมากกว่า
	 - **Linear Regression:** ไม่เหมาะสมเนื่องจากเป็นงานด้าน Classification ไม่ใช่การพยากรณ์ตัวเลข
	 - **K-Means:** ไม่ถูกเลือกเนื่องจากเป็นการจัดกลุ่มแบบ Unsupervised Learning ซึ่งไม่สามารถระบุประเภทเห็ดพิษได้อย่างชัดเจน



**ผลลัพธ์และการนําไปใช้งาน**

 - ข้อมูลเชิงลึก
	 - กลิ่น (Odor) เป็นตัวแปรที่สําคัญที่สุด โดยเห็นที่มีกลิ่น Almond หรือ Anise มีแนวโน้มที่จะกินได้ ส่วนเห็ดที่มีกลิ่นฉุนหรือเหม็นมักจะมีพิษ
	 - สีของสปอร์ (Spore Print Color) เป็นอีกปัจจัยสําคัญที่ช่วยในการตัดสินใจ
    
![Mushroom_Result](https://drive.google.com/file/d/1INoBgq1eKCzqZCLKVQScowghxeg9ltoB/view?usp=drive_link)

**เครื่องมือที่ใช้:** RapidMiner
**วิดิโอนําเสนอ:** [https://www.youtube.com/watch?v=tWEDENedrAg](https://www.youtube.com/watch?v=tWEDENedrAg)
