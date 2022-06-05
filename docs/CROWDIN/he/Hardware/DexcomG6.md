# Dexcom G6

## ראשית דבר

-   עקבו אחר ההמלצה הכללית על היגיינה והגדרת חיישן [כאן](../Hardware/GeneralCGMRecommendation.md).
-   עבור משדרי G6 שיוצרו לאחר סתיו/סוף 2018 נא הקפידו להשתמש באחת מגרסאות -xDrip+ החדשות ביותר</a>. למשדרים האלה יש קושחה חדשה שאיתה הגרסה היציבה העדכנית  ל-10/01/2019 לא יכולה להתמודד.

## הנחיות כלליות ללופ עם G6

השימוש ב-G6 קצת יותר מורכב ממה שנדמה. כדי להשתמש בו בבטחה, יש לשים לב למספר נקודות:

-   אם אתם משתמשים בנתונים הנאטיביים (Native Data) עם קוד הכיול ב-xDrip+ או ב-Spike, הדבר הבטוח ביותר שאפשר לעשות הוא לא לאפשר הפעלה מחדש של החיישן.
-   אם אתם מוכרחים להשתמש באתחול מונע, הקפידו להפעיל זאת בזמן ביום בו תוכלו לצפות בשינוי ולכייל במידת הצורך.
-   אם אתם מפעילים מחדש חיישנים, בצעו זאת ללא כיול המפעל על מנת לקבל את התוצאות הבטוחות ביותר בימים 11 ו -12, או וודאו שאתם מוכנים לכייל ולשים עין על סטיות.
-   "השרייה" מוקדמת של ה-G6 (הדבקת חיישן מבלי להפעילו למספר שעות) עם כיול המפעל עשויה לגרום סטיה בתוצאות. אם אתם עושים השרייה מוקדמת, כדי לקבל את התוצאות הטובות ביותר, סביר להניח שתצטרכו לכייל את החיישן.
-   אם אינכם מקפידים לפקח על השינויים שעלולים להתרחש, אולי עדיף לחזור למצב שאינו מכויל ע"י היצרן ולהשתמש במערכת כמו ב-G5.

למידע נוסף על הפרטים והסיבות להמלצות אלה קראו את [המאמר המלא](https://www.diabettech.com/artificial-pancreas/diy-looping-and-cgm/) שפרסם טים סטריט בכתובת [www.diabettech.com](https://www.diabettech.com).

## אם משתמשים ב-G6 עם xDrip+

-   ניתן לחבר בו-זמנית משדר דקסקום G6 למקלט דקסקום (או לחילופין את המשאבה t:slim) ואפליקציה אחת בטלפון.
-   בעת שימוש ב-xDrip+ כמקלט הסירו תחילה את אפליקציית דקסקום. **לא ניתן לחבר את xDrip+ וגם את אפליקציית דקסקום למשדר בו-זמנית!**
-   אם אתם צריכים להשתמש ב-Clarity ורוצים ליהנות מהתראות ש-xDrip+ מציע, השתמשו ב-[BYODA - "בנה אפליקציית דקסקום בעצמך" ](../Hardware/DexcomG6#if-using-g6-with-build-your-own-dexcom-app), המציעה שידור מקומי ל-xDrip+.
-   אם עדיין לא הוגדר אז הורידו [xDrip+](https://github.com/NightscoutFoundation/xDrip) ועקבו אחר הוראות [הגדרת xDrip+](../Configuration/xdrip.md).
-   בחרו ביישום xDrip+ בבונה התצורה (הגדרה ב- AndroidAPS).
-   התאימו את ההגדרות ב-xDrip+ לפי [דף הוראות   xDrip+](../Configuration/xdrip.md)
-   אם AAPS אינו מקבל ערכי סוכר כאשר הטלפון במצב טיסה השתמשו ב'זהה מקלט' כפי שמתואר בהגדרות [xDrip+ דף](../Configuration/xdrip.md).

## אם משתמשים ב-G6 עם Build Your Own Dexcom App (BYODA)

-   החל מדצמבר 2020 [בנה אפליקציית Dexcom בעצמך (BYODA)](https://docs.google.com/forms/d/e/1FAIpQLScD76G0Y-BlL4tZljaFkjlwuqhT83QlFM5v6ZEfO7gCU98iJQ/viewform?fbzx=2196386787609383750&fbclid=IwAR2aL8Cps1s6W8apUVK-gOqgGpA-McMPJj9Y8emf_P0-_gAsmJs6QwAY-o0) תומך גם בשידור מקומי ל-AAPS ו\או ל-xDrip+ (לא עבור חיישני G5!)
-   אפליקציה זו מאפשרת להשתמש ב-Dexcom G6 עם כל סמארטפון אנדרואיד.
-   הסירו את ההתקנה של אפליקציית Dexcom המקורית או אפליקציית Dexcom עם פאץ' אם השתמשתם באחת מאלה בעבר.
-   התקינו את קובץ ה-APK של יישום שהורדתם
-   הזינו את קוד החיישן ואת המספר סידורי של המשדר.
-   בהגדרות הטלפון, נווטו אל אפליקציות > Dexcom G6 > הרשאות > הרשאות נוספות ולחצו על 'גישה לאפליקציית Dexcom'.
-   לאחר זמן קצר BYODA אמור לקלוט את אות המשדר. (אם לא, תצטרכו לעצור את החיישן ולהתחיל אחד חדש)

### הגדרות לשימוש עם AndroidAPS

-   בחרו "אפליקציית Dexcom עם פאץ'" בבונה התצורה.
-   אם אינכם מקבלים ערכים כלשהם, בחרו מקור נתונים אחר,   אז בחר מחדש את 'אפליקציית Dexcom (מעודכנת)' כדי להפעיל את   ההרשאה עבור הרשאות ליצור את הקשר בין AAPS   ושידור BYODA.

### הגדרות עבור xDrip+

-   בהגדרות xDrip בחרו '640G/Eversense' כמקור נתונים.
-   יש לבחור את הפקודה "התחל חיישן" ב-xDrip+ על מנת   לקבל ערכים. זה לא ישפיע על חיישן הדקסקום הנוכחי כי הוא נשלט על ידי   Build Your Own Dexcom App בלבד.

## פתרון בעיות G6

### פתרון בעיות ספציפיות של Dexcom G6

-   משדרים שמספריהם מתחילים ב-80 או 81 צריכים גרסת xDrip+ היציבה האחרונה ממאי 2019 או גרסת בנייה לילית חדשה יותר.
-   משדרים שמספריהם מתחילים ב-8G צריכים גרסת xDrip מבנייה לילית מ-25 ביולי 2019 או מאוחר יותר.
-   לא ניתן לחבר את האפליקציה xDrip+ ו-Dexcom עם המשדר בו-זמנית.
-   המתינו לפחות 15 דקות בין עצירה להפעלת חיישן.
-   אין להריץ אחורה את זמן ההחדרה. ענו על השאלה   "הכנסתם את החיישן היום?" עם "כן, היום", תמיד.
-   אל תפעילו "הפעל מחדש חיישנים" בזמן הגדרת חיישן חדש
-   אין להפעיל חיישן חדש לפני שהמידע הבא מוצג בדף הסטטוס הקלאסי > מצב G5/G6 < PhoneServiceState:
    -   משדר עם מספר סידורי המתחיל ב- 80 או 81: "Got data hh:mm"   (לדוגמה: "Got data 19:04")
    -   משדרים עם מספרים סידוריים המתחילים ב-8G, 8H, 8J וכו': "Got glucose hh:mm" (לדוגמה: "Got glucose 19:04") או "Got no raw hh:mm" (לדוגמה: "Got no raw 19:04")

![xDrip+ PhoneServiceState](../images/xDrip_Dexcom_PhoneServiceState.png)

### פיתרון בעיות כלליות

פתרון בעיות כלליות עבור חיישנים ניתן למצוא [כאן](./GeneralCGMRecommendation#troubleshooting).

### משדר חדש עם חיישן שכבר מופעל

במקרה של החלפת משדר במהלך פעילות חיישן שכבר הופעל, תרצו להסיר את המשדר מבלי לפגוע בחיישן. ניתן למצוא סרטון הדגמה בכתובת <https://youtu.be/tx-kTsrkNUM>.