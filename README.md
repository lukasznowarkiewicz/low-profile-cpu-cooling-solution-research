# CPU low profile cooling solutions research  
  
  
## Overview  
  
  
Cooling a device while keeping the overall thickness low can be a real challenge. I ran into this problem while looking at compact systems based on SOM modules.  
  
With COM Express, the total stacking height of the PCB sandwich is already more than 1 cm, and with components it can easily reach 1.5 cm. SMARC modules with a straddle-mount connector can reduce that height to only a few millimeters. But then another problem shows up: how to cool it.  
  
For a 15 W maximum dissipated power processor, a 2 mm thick heat spreader that is expected to work across the industrial temperature range looks unrealistic from the start. The most reasonable approach, without increasing the total device thickness, is to move the heatsink away from the CPU and make it larger. But then several practical questions appear:  
  
- Can this be done with simple copper bars?  
- Is a heat pipe or vapor chamber a better solution?  
- Over what distance can heat be transferred effectively?  
- How much power can such a setup handle?  
  
To get some first answers, I decided not to start with simulation, but with real hardware experiments. The approach was simple: build something quickly, test it, and improve it later.  
  
As a test platform I chose the LattePanda Mu, because it is easy to obtain, has COTS cooling accessories available, and has broadly available documentation, including 3D CAD models. The plan was to buy several COTS heat-transfer elements, attach them to the SOM CPU using a custom mounting solution, and compare how they behave in practice.  
  
**Why not simulation?**  
  
Thermal simulation is useful, but in this case there is an important factor that is difficult to model well: CPU behavior close to thermal limits.  
  
Modern Intel CPUs, as well as CPUs from other vendors, implement protection mechanisms against thermal runaway. When the junction temperature gets close to the limit, the CPU reduces clock speed and power consumption to protect itself. That means the dissipated power is not constant — it depends on temperature and on the internal thermal control behavior of the processor.  
  
Because of that, a purely simulated model would miss an important part of the real system behavior.  
  
**About the test quality**  
  
These experiments are not meant to be fully scientific or fully controlled. Many simplifications were made, for example:  
  
- only one run was performed for each test  
- the software BOM was not fully tracked  
- exact BIOS revision, CPU microcode version, and Windows build were not recorded  
- starting conditions were not perfectly identical between runs  
  
The goal of this work was not to produce precise laboratory-grade data, but to gain **rough estimates** and **practical understanding** of how different cooling elements behave in a low-profile CPU cooling setup.  
  
  
## Test setup:  


### Overview  
  
  
  
For all of the upcoming experiments, the overall idea was to split the cooling solution into three main parts:  
  
- **CPU block** — takes heat away from the CPU and allows attaching different heat-transfer elements  
- **Heat-transfer element** — transfers heat away from the CPU toward the heatsink  
- **Heatsink** — exchanges heat with the ambient air  
  
This split made it easier to test different combinations while keeping the setup modular. Instead of redesigning the whole cooling solution each time, I could change only one part and observe how it affected the overall behavior.  
  
The main focus of the experiments was to compare:  
  
- different heatsinks  
- different heat-transfer elements  
- different fan control profiles  
  
This approach does not represent a final product-ready cooling design. It is a practical test platform built to quickly evaluate what kinds of low-profile cooling solutions make sense and which directions are worth developing further.  
  
  
### Lattepanda MU  
  
Lattepanda MU is equipped with Intel N100 processor with 6W TDP. For the experiment removed original cooler, cleaned original thermal paste and applied AG TermoPasty Gold  thermal paste onto it. Running windows 11 and Cinebench R20 for stress testing.  
  
  
The LattePanda Mu is equipped with an Intel N100 processor with a 6 W TDP. For the experiments, the original cooler was removed, the original thermal paste was cleaned off, and AG TermoPasty Gold thermal paste was applied ([buy link](https://www.tme.eu/pl/details/pasta-gold-100/pasty-termoprzewodzace/ag-termopasty/art-agt-119/)). The system was running Windows 11, [Cinebench R20](https://cinebench.net) and [Web Pasemark](https://web.basemark.com) were used for stress testing.  
  
![LATTEPANDA,](Attachments/CE7677DC-B362-4CB1-9059-F407451537B7.jpeg)  
  
### CPU block CAD Design  
  
To attach different heat transferring elements to the CPU had to design custom mounting bracket. For this designed cooper block with threaded holes for mounting it to the Latetpanda Mu and attaching bracket holding the heat transferring elements. Lattepanda MU model from the official repository: [https://github.com/LattePandaTeam/LattePanda-Mu](https://github.com/LattePandaTeam/LattePanda-Mu)  
  
  
![Screenshot 2026-03-08 at 18.51.22.png](Attachments/4DEA3AB1-E3AD-4564-B9EF-535970F65FFF.png)  
  
The CPU block consists of a few components:  
- copper block — transfers heat from the CPU to the heat pipe or vapor chamber; it also serves as the main mechanical part, mounted to the PCB, with the remaining parts attached to it  
- sheet metal part — holds the heat-transfer element against the block  
- mounting screws with springs — provide firm mounting to the CPU while still allowing for thermal expansion  
  
![B70504AD-3775-4B64-B8EC-7B879E40D5D9_1_201_a.heic](Attachments/292FC026-CE91-4265-93AF-48DFB1588D12.heic)  
  
![Image](Attachments/C8C2ABC2-83A4-41BA-8D38-65C9F6A77DA2.jpeg)  
  
  
### Various COTS heat transferring element  
  
Analyzing the [thermal conductivity of different materials](https://thermtest.com/thermal-resources/materials-database), it is easy to see that copper and aluminium are among the best metals for heat-transfer purposes. Of course, silver and gold are also excellent, but for this use case the first two are probably much more economically justified. A few COTS elements were selected that were relatively easy to buy, in different lengths, to test how far the heatsink could be placed. For easier identification, each one was marked with a sticker.  
  

- DUT A001 - 1x120x9mm copper flat bar [buy link](https://www.aliexpress.com/item/1005010187708158.html?spm=a2g0o.order_detail.order_detail_item.5.558743ceJzACP9)  
  
- DUT A002 - 2.5x120x8.4mm copper heat pipe [buy link](https://www.aliexpress.com/item/1005010187708158.html?spm=a2g0o.order_detail.order_detail_item.5.1f9543ceo8XzEu)  
  
- DUT A005 - 120X26X2mm aluminium vapor chamber [buy link](https://www.aliexpress.com/item/1005004067862649.html?spm=a2g0o.order_detail.order_detail_item.5.4b3e43ceqU9YxJ)  
  
- DUT A003 - 2.5x180x8.4mm copper heat pipe [buy link](https://www.aliexpress.com/item/1005010187708158.html?spm=a2g0o.order_detail.order_detail_item.3.558743ceJzACP9)  
  
- DUT A006 - 180X26X2mm aluminium vapor chamber [buy link](https://www.aliexpress.com/item/1005004067862649.html?spm=a2g0o.order_detail.order_detail_item.3.4b3e43ceqU9YxJ)  
  
- DUT A004 - 2.5x240x8.4mm copper heat pipe [buy link](https://www.aliexpress.com/item/1005010187708158.html?spm=a2g0o.order_detail.order_detail_item.3.1f9543ceo8XzEu)  
  
- DUT A007 - 240X26X2mm aluminium vapor chamber [buy link](https://www.aliexpress.com/item/1005004067862649.html?spm=a2g0o.order_detail.order_detail_item.3.260843ce2q29eo)  
  
  
![Image](Attachments/92272AF4-074D-4ABB-AFD8-FAAC6288DBD3.jpeg)  
  
  
### Various COTS heatsinks  
  
For the heatsinks pick a few made from aluminum, two made of cooper and one active cooler:  
  
- DUT B001 - 40x40x11mm aluminium heatsink [buy link](https://www.aliexpress.com/item/1005008649776130.html?spm=a2g0o.order_detail.order_detail_item.3.67fa43cedomBAL)  
  
- DUT B002 - 40x40x20mm aluminium heatsink ([buy link](https://www.aliexpress.com/item/1005008649776130.html?spm=a2g0o.order_detail.order_detail_item.3.67fa43cedomBAL)) with two 15x15x1.5mm copper plates underneath ([buy link](https://www.aliexpress.com/item/10000033780432.html?spm=a2g0o.order_detail.order_detail_item.3.364343ceTm0lcQ)), connected with thermal paste AG TermoPasty Gold ([buy link](https://www.tme.eu/pl/details/pasta-gold-100/pasty-termoprzewodzace/ag-termopasty/art-agt-119/))   
  
- DUT B003 - 40x40x11mm copper heatsink [buy link](https://www.aliexpress.com/item/1005010303869965.html?spm=a2g0o.order_detail.order_detail_item.3.674043ceZhvzXI)  
  
- DUT B004 - 5.5x28x65mm copper flat heat sink [buy link](https://www.aliexpress.com/p/order/detail.html?spm=a2g0o.order_list.order_list_main.96.29151c24wB8yOk&orderId=3067847133937217)  
  
- DUT B005 - DFRobot FIT0982 aluminium thin aluminium heat sink for Lattepanda MU [buy link](https://www.dfrobot.com/product-2824.html?srsltid=AfmBOor6i_Rqx-vEuyuZHIeGtHzAXB5v5sFsKusfg6E_pYEmTbm0hqfd)  
  
- DUT B006 - DFRobot FIT0981 aluminium active cooler for Lattepanda MU [buy link](https://www.dfrobot.com/product-2823.html)  
  


  
![Image](Attachments/B84309C3-85E5-4B87-ABA1-B34D2A856287.jpeg)  
  
![Image](Attachments/C7ECD265-87C9-4FF5-84D1-6EECF9A590F2.jpeg)  
  
  
### Complete test setup  
  
![Image](Attachments/4DF6DC36-E353-424A-9A2C-0EFAAE12CC71.jpeg)  
  
  
  
## Experiments  
  
### Overview  
  
To test the various solutions while limiting the number of variables, the experiments were reduced to three basic groups:  
  
1. performance of various heatsinks  
2. performance of various heat-transfer elements  
3. experiments with fine-tuning the fan profile  
  
  
### 1st series test - various heatsinks  
  
Constants in the test:  
- Ambient temperature  
- DUT A002 x2 as heat transferring element  
  
Variables:  
- Heatsinks B001-B006  
  
Recorded values:  
- Cinebench R20 result  
- CPU temperature during test  
- CPU clock during test  
- Heat distribution (with thermal camera)  
  
  
**Heatsink under test: B001**  
  
![Image](Attachments/2978DE6F-B9C6-4AE8-9046-976DB3207CF4.jpeg)  
  
![Image](Attachments/Ser1%20DUTA002x2%20DUTB001.gif)

Cinebench result: 681 pts  
  
![Screenshot 2026-03-08 at 20.07.55.png](Attachments/ECAF4F52-52D8-49FD-8EDC-6D83EC63D27C.png)  
  
![Screenshot 2026-03-08 at 20.08.27.png](Attachments/C1FBF3AE-2A7B-4E2D-93FA-57BFB507ED6D.png)  
  
  
**Heatsink under test: B002**  
   
![Image](Attachments/D9DEED63-5BF9-4D37-8921-D779220F10F7.jpeg)  
  
![Image](Attachments/Ser1%20DUTA002x2%20DUTB002.gif)

Cinebench result: 621 pts  
  
![Screenshot 2026-03-08 at 20.26.52.png](Attachments/A6DEDDBF-182E-4AF8-935D-7A7CFC1F6E89.png)  
  
![Screenshot 2026-03-08 at 20.27.21.png](Attachments/A185559E-4183-4CFD-84C4-2094C64296D0.png)  
  
**Heatsink under test: B003**  
   
![Image](Attachments/F16C9932-42EA-430C-A5F8-C2B1CCE4680B.jpeg)  
  
![Image](Attachments/Ser1%20DUTA002x2%20DUTB003.gif)

Cinebench result: 676 pts  
  
![Screenshot 2026-03-08 at 20.31.50.png](Attachments/B5DE8CDC-A48D-4D28-BADE-3A6D1C18FFF6.png)  
  
![Screenshot 2026-03-08 at 20.32.13.png](Attachments/DBE60109-8BD3-4A13-BDE6-FCACA0F87AD9.png)  
  
**Heatsink under test: B004**  
   
![Image](Attachments/AD9D67A9-CE84-4F69-B8A6-D3079348D2DB.jpeg)  
  
![Image](Attachments/Ser1%20DUTA002x2%20DUTB004.gif)

Cinebench result: 441 pts  
  
![Screenshot 2026-03-08 at 20.34.21.png](Attachments/2238E1C8-E3F1-48F2-BECF-9A75C32AED40.png)  
  
![Screenshot 2026-03-08 at 20.34.56.png](Attachments/DDFF560D-44A9-487D-AD26-C00B877F5732.png)  
  
  
**Heatsink under test: B005**  
   
![Image](Attachments/3D017678-8A83-4EB7-84AD-2F48403718A3.jpeg)  
  
![Image](Attachments/Ser1%20DUTA002x2%20DUTB005.gif)

Cinebench result: 838 pts  
  
![Screenshot 2026-03-08 at 20.36.27.png](Attachments/E19A0C1F-9B31-465F-B32C-8673061F2964.png)  
  
![Screenshot 2026-03-08 at 20.36.45.png](Attachments/E30C0829-5C1F-4185-AB67-DFDEAB57949E.png)  
  
  
  
**Heatsink under test: B006**  
   
![Image](Attachments/20892FBB-790C-46E9-9603-4C5CD6741ED7.jpeg)  

![Image](Attachments/Ser1%20DUTA002x2%20DUTB006.gif)
  
Cinebench result: 979 pts  
  
![Screenshot 2026-03-08 at 20.42.47.png](Attachments/D2BE096C-DBC6-4242-AB8F-D71294231158.png)  
   
![Screenshot 2026-03-08 at 20.41.26.png](Attachments/5D0DB956-5656-4A7E-82AF-5066A2CFCC5D.png)  
  
  
**Tests summary:**  
  

| DUT | Duration [mm:ss] | cinebench [points] | minimum clock [GHz] | Max temperature [deg C] |
| ---- | ---------------- | ------------------ | ------------------- | ----------------------- |
| B001 | 8:24 | 681 | 1.65 | 92 |
| B002 | 8:22 | 621 | 1.72 | 92 |
| B003 | 8:44 | 676 | 1.55 | 87 |
| B004 | 11:58 | 441 | 1.15 | 87 |
| B005 | 6:20 | 838 | 2.16 | 92 |
| B006 | 4:44 | 979 | 2.82 | 87 |
  
  
  
  
  
### 2nd series test - various heat transferring elements  
  
Constants in the test:  
- Ambient temperature  
- DUT B006 as a heatsink  
  
Variables:  
- Heat transferring elements A001-A007 1 pc or 2 pcs  
  
Recorded values:  
- Cinebench R20 result  
- CPU temperature during test  
- CPU clock during test  
- CPU power consumption  
- Heat distribution (with thermal camera)  
  
  
**Heat transferring element under test: A001 x1**  
  
![Image](Attachments/DA127582-E9B0-42E8-A076-7ECE7390AAB6.jpeg)  

![Image](Attachments/Ser2%20B006%20A001x1.gif)
   
Cinebench result: 944 pts  
  
![Screenshot 2026-03-08 at 22.04.35.png](Attachments/B9A72F86-2CD2-4E22-BFC1-2925A6894792.png)  
  
![Screenshot 2026-03-08 at 22.04.51.png](Attachments/6CD5D685-8A20-432C-BFE9-3B4B27D85ED1.png)  
  
![Screenshot 2026-03-08 at 22.05.17.png](Attachments/F1BB8105-F4B8-4FE1-B74F-E2A4B57A07D3.png)  
  
  
**Heat transferring element under test: A001 x2**  
  
![Image](Attachments/71A846A6-8DF2-42F7-B6C3-7C4A6DEA4A6E.jpeg)  
   
![Image](Attachments/Ser2%20B006%20A001x2.gif)

Cinebench result: 988 pts  
  
![Screenshot 2026-03-08 at 22.07.27.png](Attachments/AF4976E5-837C-4902-9049-BD067979041F.png)  
  
![Screenshot 2026-03-08 at 22.07.49.png](Attachments/BE90EC82-FF48-4473-B986-7C5B45C50DA1.png)  
  
![Screenshot 2026-03-08 at 22.08.06.png](Attachments/49F758BD-D5FF-44A4-9882-300F1AF29AA1.png)  
  
  
**Heat transferring element under test: A002 x1**  
  
![Image](Attachments/EE6ECA19-4C68-45BD-9FD8-927FAD5D5FF5.jpeg)  
   
![Image](Attachments/Ser2%20B006%20A002x1.gif)

Cinebench result: 952 pts  
  
![MT](Attachments/6F45ED77-F50E-4945-A0DD-C54E5D0BE19C.png)  
  
![Screenshot 2026-03-08 at 22.12.27.png](Attachments/0AB091AF-92F8-47F6-8A13-3D582D8B9D21.png)  
  
![Screenshot 2026-03-08 at 22.12.58.png](Attachments/CBEA43B7-A127-4853-930F-3AECAE45853D.png)  
  
**Heat transferring element under test: A002 x2**  
  
![Image](Attachments/8EFFFC55-3C25-4BD4-9F62-D57C09FE8077.jpeg)  

![Image](Attachments/Ser2%20B006%20A002x2.gif)
   
Cinebench result: 943 pts  
  
![Screenshot 2026-03-08 at 22.14.12.png](Attachments/CCBF3F92-938A-4ED5-8FAD-17C3B2D6C28D.png)  
  
![Screenshot 2026-03-08 at 22.14.31.png](Attachments/F18B12B1-5A81-46AC-9160-77C7462EDCE0.png)  
  
![Screenshot 2026-03-08 at 22.15.16.png](Attachments/640B8B7B-60D2-4A57-812D-81B7FBC8B43B.png)  
  
**Heat transferring element under test: A005 x1**  
  
![Image](Attachments/CEE6E8F6-0D89-48E7-82EF-9135BAFB8D86.jpeg)  

![Image](Attachments/Ser2%20B006%20A005x1.gif)
   
Cinebench result: 782 pts  
  
![Screenshot 2026-03-08 at 22.16.18.png](Attachments/7BCCEDD2-C01E-42BF-AB34-C44B6A9E2EB6.png)  
  
![Screenshot 2026-03-08 at 22.16.37.png](Attachments/FB71DCE9-EA57-481D-BC5E-1C83F6089F80.png)  
  
![Screenshot 2026-03-08 at 22.16.54.png](Attachments/5BB08232-EDCB-42E5-B61F-F5B14C6945AF.png)  
  
  
**Heat transferring element under test: A003 x1**  
  
![Image](Attachments/C3791D5E-5099-469E-B307-FB32505BC6E2.jpeg)  

![Image](Attachments/Ser2%20B006%20A003x1.gif)
   
Cinebench result: 930 pts  
  
![Screenshot 2026-03-09 at 07.30.00.png](Attachments/C0504EEA-5F2C-4D5E-8099-2E9775606D8B.png)  
  
![Screenshot 2026-03-09 at 07.29.39.png](Attachments/50E9282E-838B-4EDD-B69C-3840FE98C26E.png)  
  
![Screenshot 2026-03-09 at 07.29.22.png](Attachments/3B0090CB-085D-4E49-BADD-5FD34368EE66.png)  
   
  
**Heat transferring element under test: A003 x2**  
  
![Image](Attachments/68C3D541-D1FC-4B01-B8B4-BF47ADB88BC6.jpeg)  
   
![Image](Attachments/Ser2%20B006%20A003x2.gif)

Cinebench result: 853 pts  
  
![Screenshot 2026-03-09 at 07.30.45.png](Attachments/A7BC36F6-1E5A-4FCB-BFCA-6CBD2532D5A9.png)  
  
![Screenshot 2026-03-09 at 07.31.04.png](Attachments/1B3A876B-EE18-4CE7-9937-9FAC2D91FA4D.png)  
  
![Screenshot 2026-03-09 at 07.31.30.png](Attachments/259BE6A7-FD41-4B9E-9BAC-2E03B55ABFB4.png)  
  
  
  
  
**Heat transferring element under test: A006 x1**  
  
![Image](Attachments/0CAD5A3F-CC0B-4D7C-8DB1-23B8B78190A0.jpeg)  

![Image](Attachments/Ser2%20B006%20A006x1.gif)
   
Cinebench result: 751 pts  
  
![Screenshot 2026-03-09 at 07.32.04.png](Attachments/46CE6498-799C-49CB-BBFC-D4934DF1A671.png)  
  
![Screenshot 2026-03-09 at 07.32.26.png](Attachments/4CDC2CF7-6CC3-488B-B23C-1B044502115A.png)  
  
![Screenshot 2026-03-09 at 07.32.39.png](Attachments/C911EEA9-3BFB-4793-A19C-0C712A39B72A.png)  
  
  
**Heat transferring element under test: A004 x1**  
  
![Image](Attachments/73E6A15E-9FEB-441C-8BEF-45FEAE89AD4B.jpeg)  
   
![Image](Attachments/Ser2%20B006%20A004x1.gif)

Cinebench result: 858 pts  
  
![Screenshot 2026-03-09 at 07.33.04.png](Attachments/DDDA396B-2E3D-4FC4-9B7D-11A15DA700CD.png)  
  
![Screenshot 2026-03-09 at 07.33.24.png](Attachments/60C18ECE-850A-467A-9CF0-80DF14CD5DE7.png)  
  
![Screenshot 2026-03-09 at 07.33.39.png](Attachments/F921A40F-FD60-480F-9F2F-4479DB05198D.png)  
  
  
**Heat transferring element under test: A004 x2**  
  
![Image](Attachments/B52C8827-B1C1-454E-95AE-9CEAD6D8F5E6.jpeg)  

![Image](Attachments/Ser2%20B006%20A004x2.gif)
  
Cinebench result: 1010 pts  
  
![Screenshot 2026-03-09 at 07.34.08.png](Attachments/F609D543-56AF-4A9B-BCAD-EB7EF9D5C2E1.png)  
  
![Screenshot 2026-03-09 at 07.34.24.png](Attachments/0F8CC78B-F664-43FF-9AC7-6CED7AFB32E1.png)  
  
![Screenshot 2026-03-09 at 07.34.41.png](Attachments/D6332119-BDC6-4A23-A885-7866AA03C55F.png)  
  
   
**Heat transferring element under test: A007 x1**  
  
![Image](Attachments/AF2B2E03-2C4C-43FB-9AFB-8CF7FC8E7356.jpeg)  
   
![Image](Attachments/Ser2%20B006%20A007x1.gif)

Cinebench result: 577 pts  
  
![Screenshot 2026-03-09 at 07.35.06.png](Attachments/74947BDC-82FD-4D2A-8867-5CBD91C2F0CB.png)  
  
![Screenshot 2026-03-09 at 07.35.19.png](Attachments/049BE34E-22FC-4189-BC6E-B8D8A83DCBDF.png)  
  
![Screenshot 2026-03-09 at 07.35.35.png](Attachments/8DE58125-F4D0-4B18-A525-45815E28EE9A.png)  
  
  
  
**Heat transferring element under test: A004 x1 + A003 x2**  
  
![Image](Attachments/554F755A-8811-45A3-B164-DA1946B7D270.jpeg)  

   
Cinebench result: 966 pts  
  
![Screenshot 2026-03-09 at 07.35.58.png](Attachments/6F2D800A-A696-4B43-92DA-0F68706B035C.png)  
  
![Screenshot 2026-03-09 at 07.38.04.png](Attachments/D1F57E27-34DC-4097-9EB0-DBA8FC29CF3D.png)  
  
![Screenshot 2026-03-09 at 07.38.22.png](Attachments/094AEA54-4D6C-442E-93C3-67076960A4E3.png)  
  
  
**Heat transferring element under test: A004 x1 + A003 x2 + A002 x2 + A004**  
  
![Image](Attachments/06B4F5F6-5CFA-4B29-835D-41A6FFF7B498.jpeg)  
  
Cinebench result: 921 pts  
  
![12:01 AM](Attachments/FA32F50A-28EB-4280-BB8C-A560934837F0.png)  
  
![Screenshot 2026-03-09 at 07.39.28.png](Attachments/6222F2F3-711B-43E0-877A-73D18C12620C.png)  
  
![Screenshot 2026-03-09 at 18.20.00.png](Attachments/66EEFEA4-B5F9-4C09-8CEB-AA66B76B3F82.png)  
  
  
**Tests summary:**  
  

| Number | Duration [mm:ss] | Cinebench [points] | Max temperature [deg C] | Minimum clock [GHz] | Minimum power [W] |
| ---------------------------------- | ---------------- | ------------------ | ----------------------- | ------------------- | ----------------- |
| A001 | 5:19 | 944 | 92 | 2.55 | 11 |
| A001 x2 | 5:08 | 988 | 92 | 2.78 | 12.5 |
| A002 | 5:17 | 952 | 92 | 2.6 | 12 |
| A002 x2 | 5:18 | 943 | 84 | 2.85 | 12.5 |
| A005 | 6:20 | 782 | 92 | 2.05 | 9 |
| A003 | 5:23 | 930 | 92 | 2.65 | 11.5 |
| A003 x2 | 5:53 | 853 | 82 | 2.78 | 13 |
| A006 | 6:40 | 751 | 92 | 1.8 | 8.5 |
| A004 | 5:51 | 858 | 92 | 2.4 | 10.6 |
| A004 x2 | 5:07 | 1010 | 85 | 2.78 | 13 |
| A007 | 8:36 | 577 | 92 | 1.4 | 7.5 |
| A004 X1 + A003 x2 | 5:28 | 966 | 80 | 2.92 | 13 |
| A004 x1 + A003 x2 + A002 x2 +
A004 | 5:27 | 921 | 89 | 2.75 | 13 |
  
  
  
### 3rd series test - various fan profiles  
  
Constants in the test:  
- Ambient temperature  
- DUT B006 as a heatsink  
- A002 + A003 x2 as heat transferring elements  
  
Variables:  
- fan profile set in fan control software [https://getfancontrol.com](https://getfancontrol.com)  
  
Recorded values:  
- Cinebench R20 result  
- Web Basemark result  
- CPU temperature during test  
- CPU clock during test  
- CPU power consumption  
- Fan noise  
  
  
![4D8E679B-A997-4A90-AC56-1BA440A13533_1_105_c.jpeg](Attachments/0380D089-47E1-4015-A483-55E1A61D9A04.jpeg)  
  
![L Fan Gortrsl](Attachments/C2D23E13-1802-4C11-A544-6E590AB74DD5.jpeg)  
  
**Fan profile under test: Profile 1 **  
  
   
![65 °C- Core Max-Ite N100](Attachments/96F46F2F-D81E-4911-836C-DD2EBAE82748.png)  
  
**Cinebench result: 906 pts**  
  
![Screenshot 2026-03-09 at 07.48.42.png](Attachments/A023B2A8-4158-4B54-9B6A-01ACB2D85436.png)  
  
![Screenshot 2026-03-09 at 07.49.48.png](Attachments/2AFA0D04-76F2-40A6-9402-9737DD6BCCA4.png)  
  
![Screenshot 2026-03-09 at 07.50.03.png](Attachments/676D1A24-8CA2-4FED-BE9D-1132895843C1.png)  
  
Sound level (dB) Over Time (mm:ss):  
  
![Screenshot 2026-03-09 at 17.56.01.png](Attachments/5C7EC099-8D5F-468D-A1B0-45CB774236D0.png)  
  
**Web passmark results: 734 pts **  
  
![Screenshot 2026-03-09 at 17.30.41.png](Attachments/B4D60BAB-A96D-4B70-90D2-6DAF0BFB1CBE.png)  
  
![Screenshot 2026-03-09 at 17.31.12.png](Attachments/CE76C4D9-4D0B-4426-9999-C9664D331176.png)  
  
Sound level (dB) Over Time (mm:ss):  
  
![0:19 0:38 0:57 1:16 1:35 1:54 2:13 2:32 2:51](Attachments/74B00A93-CFC5-495B-943A-C8E0B0580426.png)  
  
  
**Fan profile under test: Profile 2**  
  
![Profile2](Attachments/F876D00C-9DF5-47D9-B8E4-F7799E2C525D.png)  
  
**Cinebench result: 1009 pts**  
  
![• CPU Temperaturel](Attachments/FFBD340E-11A0-489E-8496-0D444B4FF5FE.jpeg)  
  
![Screenshot 2026-03-09 at 17.03.24.png](Attachments/D4ED9D01-0EB6-4CEA-AD8E-B5407A068D5C.png)  
  
![Screenshot 2026-03-09 at 17.03.40.png](Attachments/8C3259A8-1FB9-4598-A46F-3C170B762A80.png)  
  
Sound level (dB) Over Time (mm:ss):  
  
![Screenshot 2026-03-09 at 17.56.27.png](Attachments/020DC506-5A5A-420C-B6D3-67A12AA216B0.png)  
  
  
**Web passmark results: 683 pts **  
  
![Screenshot 2026-03-09 at 17.31.45.png](Attachments/8F350F66-CAE1-4E67-8FA7-6E926BC414CA.png)  
  
![Screenshot 2026-03-09 at 17.44.47.png](Attachments/D7439E47-4D4F-4E51-A875-6A6E3266D1CE.png)  
  
Sound level (dB) Over Time (mm:ss):  
  
![Screenshot 2026-03-09 at 17.54.29.png](Attachments/3BBE8B4C-B712-4FFE-982C-3941B5725825.png)  
  
**Fan profile under test: Profile 3**  
  
![Screenshot 2026-03-09 at 17.06.12.png](Attachments/0AD19EBD-A1EB-482C-BF5E-5E7178DC1FB5.png)  
  
**Cinebench result: 1009 pts**  
  
![Screenshot 2026-03-09 at 17.14.41.png](Attachments/55A7F1AE-BEDD-4E6B-BFB3-9350D444DC78.png)  
  
![Screenshot 2026-03-09 at 17.14.58.png](Attachments/016AB72B-AC4E-4437-9DCC-407EBB53E9F5.png)  
  
![Screenshot 2026-03-09 at 17.15.14.png](Attachments/30791590-6F44-4BF6-977C-D51045AA228B.png)  
  
Sound level (dB) Over Time (mm:ss):  
  
![Screenshot 2026-03-09 at 17.57.05.png](Attachments/214E5EB9-03CB-417C-B38A-B9F1CC2EB1E5.png)  
  
  
**Web passmark results: 599 pts **  
  
![Screenshot 2026-03-09 at 17.46.50.png](Attachments/F4C73C51-3D32-4181-9F7C-A407D12A6529.png)  
  
![Screenshot 2026-03-09 at 17.47.04.png](Attachments/FD1EF778-7928-4BA9-B090-1F10CDFB3515.png)  
  
Sound level (dB) Over Time (mm:ss):  
  
![Screenshot 2026-03-09 at 17.54.51.png](Attachments/6A386F56-D09C-4DB2-B5C2-9A9B0FC87954.png)  
  
  
**Fan profile under test: Profile 4**  
  
![Profile4](Attachments/BCD3F3AB-AB79-4FB9-81A2-A2170CB6D7F4.png)  
  
**Cinebench result: 978 pts**  
  
![Screenshot 2026-03-09 at 17.16.09.png](Attachments/53326CC3-FF89-4865-9562-20141C2A59EB.png)  
  
![Screenshot 2026-03-09 at 17.16.25.png](Attachments/98D150AA-8132-4B5E-9121-974BA3C24D89.png)  
  
![Screenshot 2026-03-09 at 17.16.41.png](Attachments/334A54B5-EE9B-4D99-8736-3ADFAC1A1EEE.png)  
  
Sound level (dB) Over Time (mm:ss):  
  
![Screenshot 2026-03-09 at 17.59.24.png](Attachments/64A866BF-D0F5-47C2-A3DB-6B6289C2BABF.png)  
  
  
**Web passmark results: 716 pts **  
  
![Screenshot 2026-03-09 at 17.47.27.png](Attachments/540B9E33-7D57-4783-80A4-1EB4E07C7A30.png)  
  
![Screenshot 2026-03-09 at 17.47.48.png](Attachments/5727A30E-F10E-43F0-8B03-AA35E418488A.png)  
  
Sound level (dB) Over Time (mm:ss):  
  
![Screenshot 2026-03-09 at 17.55.12.png](Attachments/61717624-4C16-486A-9000-E591D23561DC.png)  
  
  
**Fan profile under test: Profile 5**  
  
![86.6 °C - 5a average - Time](Attachments/C41FB4B4-C817-4330-BFEC-A00ECD6897AC.png)  
  
**Cinebench result: 1004 pts**  
  
![Screenshot 2026-03-09 at 17.17.33.png](Attachments/7E95CD21-CB23-4EBB-AD6F-4C2E7924DB33.png)  
  
![Screenshot 2026-03-09 at 17.17.49.png](Attachments/0011CCDA-0A24-4AB1-908A-4BB15BD2D5FD.png)  
  
![Screenshot 2026-03-09 at 17.18.05.png](Attachments/5657B210-EB1F-49AD-A66E-FE0D8B652FD6.png)  
  
Sound level (dB) Over Time (mm:ss):  
  
![Screenshot 2026-03-09 at 18.00.35.png](Attachments/B5789CC7-A6ED-4E93-9C58-3D944EA08B9B.png)  
  
  
**Web passmark results: 739 pts **  
  
![wwwww.wadeuepmanle](Attachments/FC0FA789-CE47-4B1A-B962-126754383ED5.png)  
  
![Screenshot 2026-03-09 at 17.48.33.png](Attachments/B2BF7FDB-C680-45CD-982B-6377105A9F35.png)  
  
Sound level (dB) Over Time (mm:ss):  
  
![Screenshot 2026-03-09 at 17.55.27.png](Attachments/1FDF4832-4552-4112-B795-BDA0EA52CAFB.png)  
  
**Summary:**  
  

| Number | Cinebench [points] | Max temperature [deg C] | Minimum clock [GHz] | Minimum power [W] | Average noise [dB] | Max noise [dB] | Min noise [dB] |
| --------- | ------------------ | ----------------------- | ------------------- | ----------------- | ------------------ | -------------- | -------------- |
| Profile 1 | 906 | 89 | 2.8 | 14.2 | 68.8 | 75 | 40.3 |
| Profile 2 | 1009 | 89 | 2.8 | 15 | 69.8 | 74.3 | 35.2 |
| Profile 3 | 109 | 90 | 2.78 | 15 | 69.3 | 72.6 | 41 |
| Profile 4 | 978 | 92 | 2.78 | 14.8 |  69 | 72.1 | 37.6 |
| Profile 5 | 1004 | 91 | 2.78 | 15 | 69.3 | 71.8 | 39.4 |
  
  
  

| Number | Web passmark [points] | Max temperature [deg C] | Average noise [dB] | Max noise [dB] | Min noise [dB] |
| --------- | --------------------- | ----------------------- | ------------------ | -------------- | -------------- |
| Profile 1 | 734 | 81 | 59.6 | 69.6 | 42.7 |
| Profile 2 | 683 | 82 | 53.8 | 67.8 | 35.8 |
| Profile 3 | 599 | 84 |  48 | 58.3 | 39.1 |
| Profile 4 | 716 | 86 | 47.6 | 59.8 | 38.4 |
| Profile 5 | 739 | 83 | 47.1 | 59.2 | 38.9 |
  
  
  
  
  
## Test results summary recap:  
##   
### 1st series test - various heatsinks  
  
  

| DUT | Duration [mm:ss] | cinebench [points] | minimum clock [GHz] | Max temperature [deg C] |
| ---- | ---------------- | ------------------ | ------------------- | ----------------------- |
| B001 | 8:24 | 681 | 1.65 | 92 |
| B002 | 8:22 | 621 | 1.72 | 92 |
| B003 | 8:44 | 676 | 1.55 | 87 |
| B004 | 11:58 | 441 | 1.15 | 87 |
| B005 | 6:20 | 838 | 2.16 | 92 |
| B006 | 4:44 | 979 | 2.82 | 87 |
  
  
  
  
**2nd series test - various heat transferring elements**  
  
  

| Number | Duration [mm:ss] | Cinebench [points] | Max temperature [deg C] | Minimum clock [GHz] | Minimum power [W] |
| ---------------------------------- | ---------------- | ------------------ | ----------------------- | ------------------- | ----------------- |
| A001 | 5:19 | 944 | 92 | 2.55 | 11 |
| A001 x2 | 5:08 | 988 | 92 | 2.78 | 12.5 |
| A002 | 5:17 | 952 | 92 | 2.6 | 12 |
| A002 x2 | 5:18 | 943 | 84 | 2.85 | 12.5 |
| A005 | 6:20 | 782 | 92 | 2.05 | 9 |
| A003 | 5:23 | 930 | 92 | 2.65 | 11.5 |
| A003 x2 | 5:53 | 853 | 82 | 2.78 | 13 |
| A006 | 6:40 | 751 | 92 | 1.8 | 8.5 |
| A004 | 5:51 | 858 | 92 | 2.4 | 10.6 |
| A004 x2 | 5:07 | 1010 | 85 | 2.78 | 13 |
| A007 | 8:36 | 577 | 92 | 1.4 | 7.5 |
| A004 X1 + A003 x2 | 5:28 | 966 | 80 | 2.92 | 13 |
| A004 x1 + A003 x2 + A002 x2 +
A004 | 5:27 | 921 | 89 | 2.75 | 13 |
  
  
  
  
##    
### 3rd series test - various fan profiles  
  
  

| Number | Cinebench [points] | Max temperature [deg C] | Minimum clock [GHz] | Minimum power [W] | Average noise [dB] | Max noise [dB] | Min noise [dB] |
| --------- | ------------------ | ----------------------- | ------------------- | ----------------- | ------------------ | -------------- | -------------- |
| Profile 1 | 906 | 89 | 2.8 | 14.2 | 68.8 | 75 | 40.3 |
| Profile 2 | 1009 | 89 | 2.8 | 15 | 69.8 | 74.3 | 35.2 |
| Profile 3 | 109 | 90 | 2.78 | 15 | 69.3 | 72.6 | 41 |
| Profile 4 | 978 | 92 | 2.78 | 14.8 |  69 | 72.1 | 37.6 |
| Profile 5 | 1004 | 91 | 2.78 | 15 | 69.3 | 71.8 | 39.4 |
  
  
  

| Number | Web passmark [points] | Max temperature [deg C] | Average noise [dB] | Max noise [dB] | Min noise [dB] |
| --------- | --------------------- | ----------------------- | ------------------ | -------------- | -------------- |
| Profile 1 | 734 | 81 | 59.6 | 69.6 | 42.7 |
| Profile 2 | 683 | 82 | 53.8 | 67.8 | 35.8 |
| Profile 3 | 599 | 84 |  48 | 58.3 | 39.1 |
| Profile 4 | 716 | 86 | 47.6 | 59.8 | 38.4 |
| Profile 5 | 739 | 83 | 47.1 | 59.2 | 38.9 |
  
  
  
## Observations and conclusions:  
  
- Intel TDP rating is confusing - 6W TDP processor is actually consuming 15 W of power. Maximum turbo power is not listed anywhere for Intel N100 processor. Designing heat sink for this amount of power to dissipate will result in CPU throttling just after a few second (which was easy to spot for first 4 heat sinks)  
- For this one of the smallest and most low power x86 intel CPU to keep performing under sustained load active cooling is required - none of the heatisinks tested (even the original one) were sufficient to keep the CPU operating at max frequency even at room temperature  
- Intel CPU has many protections agains poorly designed or insufficient heat sinks - it limits operating frequency to maintain safe temperature  
- Probably should work on exact starting conditions between tests as some of the experiments seems to be counterintuitive (ex. Test with 2 exactly same heatpipes came out worse than with single)  
- The best performing thermal elements are two 24 cm heat pipes (DUT A007) - I’m critical of this result - it could be because of largest thermal capacitance of the elements   
- Two raw copper bars were behaving great and those were second best result - it is a good information as raw copper bars were the thinnest and could result in overall thinest cooling solution  
- Third best result is for 3 parallel coper heatpipes - here CPU frequency seemed locked at 2.92 GHz, where temperatures were below 80 deg - that could be some variable in CPU firmware making decisions at which frequencies work  
- All aluminium vapor chambers are useless for this amount of heat needed to transfer - even single 24 cm cooper heat pipe behaves better, being much lighter and smaller  
- The best results achieved for 3 copper heatpipes and active cooled heatsink  
- Two most pleasant for the ears fan profiles were 4 and 5 - the fan was not rapidly increasing or decreasing RPM - those are having the lowest average noise  
- Fan profiles 4 and 5 were also the best ones for the performance - profile 5 achieved the best result both in cinebench - on sustained load and on web passmark test - on pulse loads. 5 seconds averaging and 2 deg hysteresis seems to be a good options   
- Why in ACPI there is not easy option to increase fan profiles based on actual power usage, just the temperature which is reactive, not proactive - controlling fan based on the power supplied to it would be better  
  
  
## Observations and conclusions  
  
- Intel TDP ratings can be misleading. A processor labeled as 6 W TDP was observed consuming up to around 15 W under load. For the Intel N100, the maximum turbo power is not clearly documented. In practice, designing a heatsink only for the nominal TDP would result in CPU throttling after just a few seconds, which was easy to observe with the first four heatsinks tested.  
  
- Even for one of the smallest and lowest-power x86 Intel CPUs, active cooling is required to maintain good sustained performance. None of the passive heatsinks tested, including the original one, were sufficient to keep the CPU operating at maximum frequency under sustained load, even at room temperature.  
  
- Modern Intel CPUs include many protection mechanisms against poorly designed or insufficient cooling solutions. Instead of overheating immediately, the CPU reduces operating frequency and power consumption to stay within safe temperature limits.  
  
- Test repeatability should be improved in future work. Some results were counterintuitive, for example cases where two identical heat pipes performed worse than a single one. This may have been influenced by different starting conditions, thermal history, mounting pressure, or measurement uncertainty.  
  
- The best-performing heat-transfer setup was two 240 mm copper heat pipes. This result should be treated with some caution, as part of the advantage may come from the larger thermal mass of the elements rather than only from better heat transfer.  
  
- For the shorter copper heat pipes, DUT A002 and A003, a single heat pipe gave better performance than two identical heat pipes combined. This seems counterintuitive at first, but calculations done using the [1-ACT heat pipe calculator](https://www.1-act.com/resources/tools/heat-pipe-calculator/) suggest that the maximum heat transferred by a heat pipe depends on the heat pipe temperature itself. It is difficult to estimate the equivalent diameter of a rounded-square profile, so reading exact values directly from the calculator is not straightforward. Additionally, the calculator is provided by a reliable heat pipe supplier, while the heat pipes used in these experiments do not have comparable documentation, so a direct numerical comparison would not be meaningful. Still, the overall trend seems clear: a heat pipe operating at a higher temperature may be capable of transferring more heat. Another important point is that for devices expected to operate below 0 °C, heat pipes may significantly reduce the cooling capability of the system.  
  
  
![LIMITS FOR COPPER/WATER HEAT PIPE, 12CM LONG, 6CM](Attachments/9E9F945A-8EDF-4BB1-B36A-452B04F4BB62.png)  
  
![LIMITS FOR COPPER/WATER HEAT PIPE, 24CM LONG, 6CM](Attachments/BE443D98-F7BE-450A-9558-FCBDAFF2AD97.png)  
- Two raw copper bars also performed very well and gave the second-best result. This is an important practical finding, because copper bars were also the thinnest solution tested and could be a good option when the total cooling stack height must be kept as low as possible. Here no surprise - two copper bars are performing better than one.  
  
- Another strong result was achieved with three parallel copper heat pipes. In that configuration, the CPU frequency appeared to stay capped at around 2.92 GHz even though the temperature remained below 80 °C. This suggests that the limitation was not thermal, but likely caused by some internal firmware or power-management threshold that prevented the CPU from reaching a higher operating point.  
  
- The aluminium vapor chambers performed poorly in these tests. For the amount of heat that needed to be transferred, even a single 240 mm copper heat pipe behaved better while also being lighter and smaller.  
  
- The best overall cooling results were achieved with multiple copper heat pipes combined with an actively cooled heatsink. This was the only class of solution that consistently kept temperatures lower while maintaining higher benchmark performance.  
  
- The heatsink itself still mattered a lot. Even with the same heat-transfer path, the choice of final heatsink had a large influence on benchmark score, minimum clock, and test duration. A good transfer element cannot compensate for an undersized heatsink.  
  
- Thin passive cooling is possible, but only within clear performance limits. Once sustained CPU power gets closer to the real package power rather than the nominal TDP, throttling becomes unavoidable unless airflow is added.  
  
- Fan profile tuning had a noticeable impact not only on noise, but also on real performance. Profiles 4 and 5 sounded the most pleasant, mainly because the fan did not ramp up and down aggressively, and they also had the lowest average noise levels.  
  
- Fan profiles 4 and 5 also gave the best performance results. Profile 5 achieved the best result both in Cinebench, representing sustained load, and in Web PassMark, representing more burst-like loads. A setting of 5-second averaging and 2 °C hysteresis seems to be a best compromise between responsiveness, acoustics, and performance.  
  
- The results suggest that fan control based only on temperature is inherently reactive. Controlling fan speed based on estimated CPU power in addition to temperature could potentially improve behavior, because it would allow the fan to respond earlier to incoming load instead of waiting for the thermal mass to heat up.  
  
- Overall, the experiments show that low-profile CPU cooling is possible, but the design window is narrow. Small mechanical changes, contact quality, transfer element choice, and fan behavior all have a visible effect on final performance.  
  
- Probably the most useful practical takeaway is that simple copper elements performed surprisingly well. Expensive or more exotic-looking solutions were not automatically better, and in some cases the simplest approach may be the most effective one.  
