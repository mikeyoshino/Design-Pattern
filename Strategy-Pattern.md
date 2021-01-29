หากใครที่เขียนโปรแกรมมาสักพักและรู้ว่าเราสามารถเขียนให้มันทำงานได้ตามที่เราต้องการ แต่พอมาดูเรื่องการเทสและการรักษาโค๊ตยังไงให้สะอาดที่สุด การเรียนรู้เรื่อง pattern ต่างๆสามารถช่วยให้เราเขียนโค๊ตได้เป็นระเบียบมากขึ้น และยังทำให้โค๊ตเราสะอาด  

ยกตัวอย่างเรามีคลาสชื่อว่า `Duck` ซึ่งเป็น `Base` คลาสสำหรับคลาสลูกๆ นั้นก็คือคลาว `Wild Duck` กับ `CityDuck` ซึ่งแน่นอนว่าเป็ดยังไงมันก็เป็นเป็ด และมีพฤติกรรมที่เหมือนกันคือ Eat ดังนั้นใน Base คลาส และคลาสลูกก็สามารถที่จะนำเอาเมธตอทนี้ไปใช้ได้ เเล้วหาก Wild Duck มันกินไม่เหมือน City Duck ละ 

ง่ายๆ เราก็ override สิแบบเป็ดป่าจะกินอะไรก็ไป implement ในคลาสตัวเองเลย 

 

# ปัญหา 

ถ้าเรามีคลาสลูกที่เพิ่มขึ้นมา และแต่ละคลาสก็มี  Eat เมทตอทที่ไม่เหมือนกันละ ? 

แล้วหากในคลาสลูกบางคลาสเช่น Mountain Duck กับ Wild Duck มี Eat เมธตอทที่เหมือนกันละ แต่การสืบทอด คลาสลูกไม่สามารถสืบทอดหรือส่งต่อ  Eat ให้กันได้ จำทำยังไงให้เราไม่ต้องเขียน เมธตอท Eat ทั้งในคลาส Mountain Duck กับ Wild Duck?   

*บางคนอาจจะคิดว่าเราก็สร้างคลาส abstruct เช่น  WildDuckEatBehavior ขึ้นมาเเล้วก็ให้ Mountain Duck กับ Wild Duck สิบทอดมาก็จบ แต่หากว่าคลาาที่มีเมธตอทที่เหมือนกันไมีมีแต่สองคลาสนี้ละ สุดท้ายก็ลงเอยที่ต้องเพิ่ม abstruct เยอะขึ้นมาเลยๆตยยุ่งไปหมด 

 

# Strategy Pattern 

ก่อนอื่นมาดูความหมายของคนคิด pattern เค้าว่ายังไง 

`Define a family of algorithms encapsulate each one and make them interchangeable. Strategy lets the algorithm vary independently from clients that use it. `

ตัวอย่างคลาส Mountain Duck กับ Wild Duck มีเมธตอทเหมือนกัน เราจะสร้าง interface ชื่อว่า IEatBehavior เเล้วมี Eat() ประกาศไว้ เเล้วก็จะมี Concret คลาสที่ Implement มธตอทเกี่ยวกับกินของเป็ดเช่น
EatVegetable กับ EatMeat อาจจะเพิ่มว่ากินอย่างอื่นได้ตามที่ต้องการ


![image info](./Images/better-error.jpg)

![image info](./Images/BlogImage/January/DynamicDualMode.png)

 

 
