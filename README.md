# Розробка SOAP-сервісів, сумісних з системою "Трембіта"
## Перелік термінів та умовних скорочень
| Термін  | Пояснення |
| :-------------: | :-------------: |
| Система електронної взаємодії державних електронних інформаційних ресурсів (система «Трембіта», система)  | Інформаційно-телекомунікаційна система, призначена для автоматизації та технологічного забезпечення обміну електронними даними між Суб’єктами електронної взаємодії з електронних інформаційних ресурсів під час надання адміністративних послуг та здійснення інших повноважень відповідно до покладених на них завдань  |
| Шлюз безпечного обміну (ШБО) | Сервер, на який встановлено програмне забезпечення UXP Security Server  |
| Unified eXchange Platform (UXP) | Програмна платформа, на базі якої побудована система «Трембіта»  |

## Вступ
Система «Трембіта» є одним із ключових елементів інфраструктури надання електронних послуг громадянам та бізнесу, який забезпечує зручний уніфікований доступ до даних електронних інформаційних ресурсів. 
Створення взаємодій між електронними інформаційними ресурсами завдяки системі «Трембіта» – процес не складний, оскільки:
-	при їх побудові використовується єдиний набір правил та форматів;
-	обмін даними в системі здійснюється через шлюзи безпечного обміну та опубліковані на них сервіси та підсистеми, які представлять електронні інформаційні ресурси.

Такий підхід не передбачає централізації даних електронних інформаційних ресурсів і зміни їх власника, а одержання доступу до даних різними установами здійснюється відповідно до попередньо заданих на ШБО повноважень, що, у свою чергу, підвищує якість надання електронних публічних послуг громадянам та бізнесу.
Також системою забезпечується високий рівень захищеності завдяки підписанню електронною печаткою та шифруванню всіх даних, що передаються, а також протоколюванню подій, контролю доступу до сервісів.
Даний документ містить відомості стосовно розробки SOAP-сервісів, сумісних з системою «Трембіта».

## Особливості виклику SOAP-сервісів з використанням системи «Трембіта»
В системі «Трембіта» електронні повідомлення-запити надсилаються наступним чином:
-	від вебклієнта до власного ШБО;
-	далі від власного ШБО до ШБО, на якому опубліковано сервіс, що представляє певний опублікований метод вебсервісу;
-	далі від ШБО, на якому опубліковано сервіс, до самого методу вебсервісу.

Процес проходження електронного повідомлення-відповіді від методу вебсервісу до вебклієнту є ідентичним й відбувається у зворотному напрямку через ШБО:

![image](https://github.com/user-attachments/assets/891dcca5-e3e5-4ad8-86a7-a29940448f8b)

На рисунку нижче в нотації IDEF0 відображено процес виклику SOAP-сервісу через систему «Трембіта», зокрема, з усіма необхідними вхідними відомостями, які для цього потрібні, а саме:
-	значенням Endpoint для виклику SOAP-сервісу – це має бути внутрішня IP-адреса клієнтського ШБО з зареєстрованою підсистемою, від імені якої буде здійснюватися виклик сервісу;
-	значенням для HTTP-заголовку SOAPAction, що потрібно видобути із завантаженого WSDL-файлу;
-	параметрами тіла SOAP-запиту (беруться з WSDL-файлу та документації на вебсервіс);
-	ідентифікатором методу вебсервісу, опублікованого на ШБО (значення даного параметру можна знайти в керівництві користувача вебсервісу або запитати безпосередньо у постачальника сервісу);
-	ідентифікатором підсистеми, опублікованої на клієнтському ШБО, від імені якої будуть здійснюватися запити (значення даного параметру можна знайти на клієнтському ШБО самостійно або запитати у адміністратора даного ШБО).

![image](https://github.com/user-attachments/assets/784ed016-43dd-4522-b839-b24746ae308d)

## Необхідні заголовки запитів для SOAP-сервісів, необхідні задля забезпечення сумісності з системою «Трембіта»
| Елементи заголовку (1 рівень вкладеності)  | Елементи заголовку (2 рівень вкладеності) | Значення атрибутів |
| :-------------: | :-------------: | :-------------: |
| client |  |  |
| | objectType | Значення за замовченням – SUBSYSTEM |
| | xRoadInstance | Середовище системи «Трембіта», SEVDEIR-TEST – для тестового та SEVDEIR – для промислового середовища |
| | memberClass | Значення за замовченням – GOV |
| | memberCode | Код ЄДРПОУ Суб’єкта електронної взаємодії, що запитує інформацію від сервісу |
| | subsystemCode | Код підсистеми, що представляє клієнт сервісу в системі «Трембіта» |
| service |  |  |
| | objectType | Значення за замовченням – SUBSYSTEM |
| | xRoadInstance | Середовище системи «Трембіта», SEVDEIR-TEST – для тестового та SEVDEIR – для промислового середовища |
| | memberClass | Значення за замовченням – GOV |
| | memberCode | Код ЄДРПОУ Суб’єкта електронної взаємодії, що запитує інформацію від сервісу |
| | subsystemCode | Код підсистеми, що представляє клієнт сервісу в системі «Трембіта» |
| | serviceCode | Код методу вебсервісу, опублікованого в системі «Трембіта» (код сервісу)  |
| | serviceVersion | Версія сервісу, не обов’язковий для заповнення атрибут |
| userId |  | Ідентифікатор особи користувача, за ініціативою якого сформовано повідомлення-запит (опціонально) |
| id |  | Унікальний ідентифікатор повідомлення-запиту у формі UUID |
| protocolVersion |  | Значення за замовченням – 4.0 |

Приклад заповнення наведено нижче:
```   <soapenv:Header>
      <xro:client iden:objectType="SUBSYSTEM">
         <iden:xRoadInstance>SEVDEIR-TEST</iden:xRoadInstance>
         <iden:memberClass>GOV</iden:memberClass>
         <iden:memberCode>11110014</iden:memberCode>
         <!--Optional:-->
         <iden:subsystemCode>SUB_prod</iden:subsystemCode>
      </xro:client>
      <xro:service iden:objectType="SERVICE">
         <iden:xRoadInstance>SEVDEIR-TEST</iden:xRoadInstance>
         <iden:memberClass>GOV</iden:memberClass>
         <iden:memberCode>11110015</iden:memberCode>
         <!--Optional:-->
         <iden:subsystemCode>88_Test_prod</iden:subsystemCode>
         <iden:serviceCode>COUNTRY</iden:serviceCode>
         <!--Optional:-->
         <iden:serviceVersion>1</iden:serviceVersion>
      </xro:service>
      <xro:userId>123</xro:userId>
      <xro:id>2346222</xro:id>
      <xro:protocolVersion>4.0</xro:protocolVersion>
   </soapenv:Header>
```

