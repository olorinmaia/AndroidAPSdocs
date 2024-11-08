# תכונות OpenAPS

(Open-APS-features-autosens)=

## Autosens (זיהוי רגישות)

* Autosens מודד סטיות של רמת הסוכר בדם (חיוביות/שליליות/נייטרליות).
* Autosens ינסה להעריך את הרגישות\תנגודת על סמך סטיות אלו.
* אלגוריתם oref ב-**OpenAPS** פועל באמצעות התחשבות בנתונים מ-24 ו-8 השעות האחרונות. הוא יבחר בנתון בו הרגישות גדולה יותר מבין חלונות הזמן הללו.
* בגרסאות שקדמו ל-AAPS 2.7 המשתמש היה צריך לבחור בין 8 או 24 שעות באופן ידני.
* מ-AAPS 2.7 ואילך, Autosens יעבור בעצמו בין חלון של 24 ל-8 שעות לחישוב רגישות. הרגיש מבינהם יבחר. 
* אם משתמשים הגיעו מ-oref1, הם כנראה ישימו לב שהמערכת עשויה להיות פחות דינמית לשינויים, בגלל המשתנה של 24 או 8 שעות של רגישות.
* החלפת צינורית או שינוי פרופיל יאפס את יחס ה-Autosens בחזרה ל-100% (החלפת פרופיל באחוזים עם משך זמן לא יאפס את Autosens).
* Autosens מתאים את המינון הבזאלי ואת יחס התיקון-ISF (כלומר: מחקה את מה שעושה שינוי פרופיל).
* אם אוכלים פחמימות לאורך זמן ממושך, Autosens יהיה פחות יעיל במהלך זמן זה מכיוון שפחמימות אינן נכללות בחישובי הפרשים של סוכר בדם.

(Open-APS-features-super-micro-bolus-smb)=

## סופר מיקרו בולוס (SMB)

SMB, הקיצור של 'סופר מיקרו בולוס', הוא האלגוריתם העדכני ביותר של OpenAPS (משנת 2018) כחלק מהאלגוריתם של Oref1. בניגוד ל-AMA, אלגוריתם SMB לא משתמש רק במינונים בזאליים זמניים כדי לשלוט ברמות הסוכר, אלא בעיקר בבולוסים קטנים **סופר מיקרו בולוסים**. במצבים שבהם AMA מוסיף 1.0 יח' אינסולין תוך שימוש במינון בזאלי זמני, SMB מספק מספר מיקרובולוסים בצעדים קטנים ב**מרווחים של 5 דקות**, למשל 0.4, 0.3, 0.2 ו-0.1 יח'. במקביל (מטעמי בטיחות) המינון הבזאלי מוגדר ל-0 יח'\שעה לתקופה מסוימת כדי למנוע מנת יתר (**'zero-temping'**). זה מאפשר למערכת להתאים את רמת הגלוקוז בדם מהר יותר מאשר עם העלייה בבזאלי הזמני ב-AMA.

בעזרת SMB, ייתכן שיספיק להודיע למערכת על כמות הפחמימות המתוכננות לארוחות המכילות רק פחמימות מורכבות ולתת ל-AAPS לאזן בעצמו. עם זאת, זה עלול להוביל לשיאים גבוהים יותר לאחר הארוחה מכיוון שלא ניתן בולוס מראש. או שמזריקים במידת הצורך **בולוס התחלתי**, שיכסה את הפחמימות **באופן חלקי** (למשל 2/3 מהכמות המשוערת) ונותנים ל-SMB להשלים את החסר.

אלגוריתם ה-SMB מכיל מספר מנגנוני בטיחות:

1. מנת ה-SMB הבודדת הגדולה ביותר יכולה להיות רק הערך הקטן ביותר מבין:
    
    * ערך המתאים למינון הבזאלי הנוכחי (כפי שמתכוונן על ידי Autosens) למשך הזמן שנקבע ב"דקות בסיס מקסימליות להגבלת SMB", למשל מינון בזאלי למשך 30 הדקות הבאות, או
    * מחצית מכמות האינסולין הנדרשת ברגע זה, או
    * החלק הנותר של ערך אינסולין הפעיל המרבי בהגדרות.

2. סביר להניח שתבחינו לעתים קרובות במינונים בזאליים זמניים נמוכים (הנקראים 'זמניים נמוכים') או במינונים בזאליים זמניים של 0 יח'\שעה (הנקראים 'אפס זמניים'). כך זה מתוכנן מטעמי בטיחות ואין לכך השפעות שליליות אם הפרופיל מוגדר נכון. לגרף האינסולין הפעיל יש יותר משמעות מאשר זה של המינונים הבזאליים.

3. חישובים נוספים לניבוי התנהגות הסוכר, למשל באמצעות UAM (ארוחות לא מוצהרות). גם ללא קלט פחמימות ידני מהמשתמש, UAM יכול לזהות באופן אוטומטי עלייה משמעותית ברמות הסוכר בעקבות ארוחות, אדרנלין או השפעות אחרות ולנסות להתאים זאת עם SMB. ליתר בטחון זה עובד גם הפוך ויכול לעצור את ה-SMB מוקדם יותר אם מתרחשת ירידה מהירה באופן בלתי צפוי בסוכר שבדם. זו הסיבה לכך שעל UAM להיות תמיד מופעל עם SMB.

**You must have started [objective 9](#objectives-objective9) to use SMB.**

ראו גם: [תיעוד OpenAPS עבור oref1 SMB](https://openaps.readthedocs.io/en/latest/docs/Customize-Iterate/oref1.html) ו[המידע של טים על SMB](https://www.diabettech.com/artificial-pancreas/understanding-smb-and-oref1/).

(Open-APS-features-max-u-h-a-temp-basal-can-be-set-to)=

### ניתן להגדיר יח'\שעה מקסימלי של בזאלי זמני (OpenAPS "max-basal")

הגדרת בטיחות זו קובעת את המינון הבזאלי הזמני המרבי שמשאבת האינסולין יכולה לספק. הערך צריך להיות זהה במשאבה וב-AAPS וצריך להיות לפחות פי 3 מהמינון הבזאלי הגבוה ביותר שנקבע בפרופיל.

לדוגמה:

המינון הבזאלי הגבוה ביותר של הפרופיל הבסיסי של במהלך היום הוא 1.00 יח'\שעה. אז מומלץ להגדיר ערך בסיסי מרבי של לפחות 3 יח'\שעה.

AAPS מגביל את הערך כ'מגבלה קשיחה' בהתאם לגיל המטופל שבחרתם בהגדרות.

AndroidAPS מגביל את הערך באופן הבא:

* ילד\ה: 2
* מתבגר\ת: 5
* מבוגר\ת: 10
* מבוגר\ת עם תנגודת אינסולין גבוהה: 12
* הריון: 25

*See also [overview of hard-coded limits](#overview-of-hard-coded-limits).*

(Open-APS-features-maximum-total-iob-openaps-cant-go-over)=

### מינון אינסולין פעיל מרבי ממנו OpenAPS לא יחרוג (maxIOB)

ערך זה קובע את ערך ה-maxIOB שתחתיו AAPS תמיד יישאר בעת פעולה במצב לולאה סגורה. אם ה-IOB הנוכחי (למשל לאחר בולוס ארוחה) הוא מעל הערך maxIOB המוגדר, הלופ יפסיק את מתן האינסולין עד שהאינסולין הפעיל יירד אל מתחת לערך ה-IOB המקסימלי.

ערך ה-maxIOB ב-OpenAPS SMB מחושב בצורה שונה מאשר ב-OpenAPS AMA. ב-AMA, ערך maxIOB הוא רק פרמטר בטיחות של IOB הנובע ממינונים בזאליים, בעוד שבמצב SMB, הוא כולל גם בולוסים וגם מינונים בזאליים. הערכה התחלתית מומלצת היא

    אינסולין פעיל מרבי = בולוס ארוחה ממוצע + מינון בזאלי מקסימלי 3X
    

היו זהירים וסבלניים ושנו את ההגדרות בהדרגה בלבד. ה-Max-IOB שונה עבור כל אחד ותלוי גם במינון היומי הממוצע (TDD). For safety reason, there is a limit, which depends on the patient age. The 'hard limit' for maxIOB is higher than in [AMA](#max-uh-a-temp-basal-can-be-set-to-openaps-max-basal).

* ילד\ה: 3
* מתבגר\ת: 7
* מבוגר\ת: 12
* מבוגר\ת עם תנגודת אינסולין גבוהה: 25
* הריון: 40

*See also [overview of hard-coded limits](#overview-of-hard-coded-limits).*

ראו גם [תיעוד OpenAPS בנושא SMB](https://openaps.readthedocs.io/en/latest/docs/Customize-Iterate/oref1.html#understanding-super-micro-bolus-smb).

### הפעלת חישוב רגישות אוטומטי (Autosens)

Here, you can choose if you want to use the [sensitivity detection](../DailyLifeWithAaps/SensitivityDetectionAndCob.md) 'autosens' or not.

(Open-APS-features-enable-smb)=

### אפשר SMB

אפשרו הגדרה זו כדי להשמש ב-SMB. אם לא תאפשרו אותה, לא יינתנו מיקרו-בולוסים.

(Open-APS-features-enable-smb-with-high-temp-targets)=

### הפעלת SMB עם ערכי מטרה גבוהים

אם הגדרה זו מופעלת, SMB יורשה, אך לא בהכרח יופעל, כאשר יש ערך מטרה זמני גבוה פעיל (מוגדר כמשהו מעל 100 מ"ג/ד"ל ללא קשר לערך המטרה של הפרופיל). אפשרות זו נועדה להשבתת SMB כאשר היא מושבתת. לדוגמה, אם אפשרות זו מושבתת, ניתן להשבית SMB על ידי הגדרת ערך מטרה זמני מעל 100 מ"ג\ד"ל. אפשרות זו גם תשבית את SMB ללא קשר לתנאי אחר שמנסה להפעיל את SMB.

אם הגדרה זו מופעלת, SMB יופעל בערך מטרה זמני גבוה רק אם מופעל גם "SMB עם ערכי מטרה גבוהים".

(Open-APS-features-enable-smb-always)=

### הפעלת SMB תמיד

אם הגדרה זו מופעלת, SMB מופעל תמיד (ללא תלות ב-COB, יעדים זמניים או בולוסים). בהפעלת הגדרה זו, שאר הגדרות ההפעלה שלהלן לא ישפיעו. עם זאת, אם "הפעל SMB עם ערכי מטרה זמניים גבוהים" מושבת ומוגדר ערך מטרה זמני גבוהה, SMB יהיו מושבתים. מטעמי בטיחות, אפשרות זו מיועדת רק למקורות נתוני סוכר עם סינון איכותי לנתונים רועשים. Currently it is only an available option with a Dexcom G5 or G6, if using the ['Build your own Dexcom App'](#DexcomG6-if-using-g6-with-build-your-own-dexcom-app) or “native mode” in xDrip+. אם בערך הסוכר יש סטייה גדולה מדי, חיישן ה-G7/G6 לא שולח אותו ללופ ומחכה לערך הבא בעוד 5 דקות.

עבור חיישנים אחרים כמו Freestyle Libre, ''השימוש ב-SMB מושבת עד של-xDrip+ יהיה תהליך החלקת רעשים טוב יותר. You can find more [here](../CompatibleCgms/SmoothingBloodGlucoseData.md).

### הפעלת SMB עם פחמ' פעילות

אם הגדרה זו מופעלת, SMB מופעל כאשר ערך הפחמימות הפעילות גדול מ-0.

### הפעלת SMB עם ערכי מטרה זמניים

אם הגדרה זו מופעלת, SMB מופעל כאשר מוגדר ערך מטרה זמני כלשהו (אכילה בקרוב, פעילות, היפו, מותאם אישית). אם הגדרה זו מופעלת אך 'הפעל SMB עם ערכי מטרה זמניים גבוהים' מושבתת, SMB יופעל כאשר מוגדר ערך מטרה זמני נמוך (מתחת ל-100 מ"ג/ד"ל) אך יושבת כאשר מוגדר יעד זמני גבוה.

### הפעלת SMB אחרי פחמימות

אם מופעל, SMB יישאר פעיל במשך 6 שעות לאחר שהוכרזו פחמימות, אפילו אם הפחמימות הפעילות ירדו ל-0. מטעמי בטיחות, אפשרות זו מיועדת רק למקורות נתוני סוכר עם סינון איכותי לנתונים רועשים. Currently it is only an available option with a Dexcom G5 or G6 if using the ['Build your own Dexcom App'](#DexcomG6-if-using-g6-with-build-your-own-dexcom-app) or “native mode” in xDrip+. אם בערך הסוכר יש סטייה גדולה מדי, חיישן ה-G7/G6 לא שולח אותו ללופ ומחכה לערך הבא בעוד 5 דקות.

עבור חיישנים אחרים כמו Freestyle Libre, ''השימוש ב-SMB מושבת עד של-xDrip+ יהיה תהליך החלקת רעשים טוב יותר. You can find [more information here](../CompatibleCgms/SmoothingBloodGlucoseData.md).

### תדירות מתן SMB (דקות)

אפשרות זו מגבילה את תדירות ה-SMB. ערך זה קובע את הזמן המינימלי בין SMB. שימו לב שהחישוב של הלולאה קורה בכל פעם שמתקבל ערך סוכר (בדרך כלל כל 5 דקות). יש להחסיר 2 דקות כדי לתת ללולאה זמן נוסף להשלמה. לדוגמה, אם תרצו ש-SMB יוזרק בכל הפעלת לולאה, הגדירו זאת ל-3 דקות.

ערך ברירת המחדל: 3 דקות.

(Open-APS-features-max-minutes-of-basal-to-limit-smb-to)=

### מקסימום הדקות של בזאלי אליו SMB מוגבל

זוהי מגבלה בטיחותית חשובה. ערך זה קובע כמה SMB ניתן לתת בהתבסס על כמות האינסולין הבזאלי בזמן נתון, כאשר הוא מכוסה על ידי פחמימות פעילות.

Making this value larger allows the SMB to be more aggressive. You should start with the default value of 30 minutes. After some experience, increase the value in 15 minutes increments and observe the effects over multiple meals.

It is recommended not to set the value higher than 90 minutes, as this would lead to a point where the algorithm might not be able to accommodate a decreasing BG with 0 U/h basal ('zero-temp'). You should also set alarms, especially if you are still testing new settings, which will warn you well before a hypo.

ערך ברירת המחדל: 30 דקות.

### הפעלת UAM

עם אפשרות זו מופעלת, אלגוריתם ה-SMB יכול לזהות ארוחות לא מוצהרות. זה מועיל אם אתם שוכחים להודיע ל-AndroidAPS על הפחמימות או אם הערכתם לא נכון את הפחמימות שהזנתם או אם בארוחה יש הרבה שומן וחלבון הגורמים למשך ספיגה ארוך מהצפוי. ללא כל הכרזת פחמימות, UAM יכול לזהות עליות מהירות של גלוקוז הנגרמות על ידי פחמימות, אדרנלין וכו', ומנסה לפצות עליהן עם SMB. זה עובד גם במקרה ההפוך: אם יש ירידה מהירה בסוכר, UAM יכול לעצור SMB מוקדם.

**לכן, UAM תמיד צריך להיות מופעל בעת שימוש ב-SMB.**

### רגישות מעלה את ערך המטרה

If this option is enabled, the sensitivity detection (autosens) can raise the target when sensitivity is detected (below 100%). In this case your target will be raised by the percentage of the detected sensitivity.

### תנגודת מורידה את ערך המטרה

If this option is enabled, the sensitivity detection (autosens) can lower the target when resistance is detected (above 100%). In this case your target will be lowered by the percentage of the detected resistance.

### ערך מטרה זמני גבוה מעלה את הרגישות

אם אפשרות זו מופעלת, הרגישות לאינסולין תוגבר כל עוד יש ערך מטרה זמני מעל 100 mg/dl. המשמעות היא שהרגישות-ISF תעלה בעוד שיחס הפחמימות-IC והמינון הבזאלי ירדו. This will effectively make AAPS less aggressive when you set a high temp target.

### ערך מטרה זמני נמוך מוריד את הרגישות

אם אפשרות זו מופעלת, הרגישות לאינסולין תפחת כשערך מטרה זמני מעל 100 mg/dl. המשמעות היא שהרגישות-ISF תפחת בעוד שיחס הפחמימות-IC והמינון הבזאלי יעלו. זה יהפוך את AAPS לאגרסיבי יותר כאשר תגדירו ערך מטרה זמני נמוך.

### הגדרות מתקדמות

**השתמש בדלתא ממוצעת קצרה במקום בנתונים פשוטים** אם תפעילו תכונה זו, AndroidAPS ישתמש בממוצע הקצר של דלתא/גלוקוז בדם מ-15 הדקות האחרונות, שהוא בדרך כלל הממוצע של שלושת הערכים האחרונים. זה עוזר ל-AndroidAPS לעבוד עם יותר יציבות כשמקורות הנתונים רועשים כמו לדוגמה xDrip+ עם Libre.

**מכפלת בטיחות בזאלי יומי מרבי/0> היא מגבלת בטיחות חשובה. הגדרת ברירת המחדל (שלא סביר שתצטרכו לשנות) היא 3. המשמעות היא ש-AndroidAPS לעולם לא יורשה להגדיר מינון בזאלי זמני שהוא יותר מפי 3 מהמינון הבזאלי השעתי הגבוה ביותר שתוכנת במשאבה של משתמש. דוגמה: אם המינון הבזאלי הגבוה ביותר בפרופיל הוא 1.0 יח'\שעה ומכפלת הבטיחות הוא 3, אז AndroidAPS יכול להגדיר מינון בזאלי זמני מרבי של 3.0 יח'\שעה (=3X1.0).</p> 

ערך ברירת המחדל: 3 (לא צריך לשנות אלא אם כן אתם באמת צריכים ויודעים מה אתם עושים)

**מכפלת בטיחות בזאלי נוכחי/0> היא מגבלת בטיחות חשובה. הגדרת ברירת המחדל (שלא סביר שתצטרכו לשנות) היא 4. המשמעות היא ש-AndroidAPS לעולם לא יורשה להגדיר מינון בזאלי זמני שהוא יותר מפי 3 מהמינון הבזאלי השעתי הגבוה ביותר שתוכנת במשאבה של משתמש.</p> 

ערך ברירת המחדל: 4 (לא צריך לשנות אלא אם כן אתם באמת צריכים ויודעים מה אתם עושים)

* * *

(Open-APS-features-advanced-meal-assist-ama)=

## סיוע ארוחה מתקדם (AMA)

AMA - "סיוע ארוחה מתקדם" היא תכונת OpenAPS משנת 2017 (oref0). המערכת יכולה להפעיל מינון בזאלי זמני גבוה מהר לאחר בולוס ארוחה אם מזינים פחמימות בצורה אמינה.

תוכלו למצוא מידע נוסף ב[תיעוד OpenAPS](https://newer-docs.readthedocs.io/en/latest/docs/walkthrough/phase-4/advanced-features.html#advanced-meal-assist-or-ama).

(Open-APS-features-max-u-hr-a-temp-basal-can-be-set-to-openaps-max-basal)=

### ניתן להגדיר יח'\שעה מקסימלי של בזאלי זמני (OpenAPS max-basal)

הגדרת בטיחות זו מכתיבה ל-AndroidAPS את המינון הבזאלי המקסימלי שאפשר להזריק ומגבילה את הקצב הבסיסי הזמני ל-x יח'\שעה. מומלץ להגדיר ערך סביר. ההמלצה היא לבחור את המינון הבזאלי הגבוה ביותר שיש בפרופיל בשעה כלשהי ביממה ולהכפיל אותו ב-4 ולכל הפחות ב-3. כך שלדוגמה, אם המינון הבזאלי הגבוה ביותר בפרופיל שלך הוא 1.0 יח'\שעה, אפשר להכפילו ב-4 כדי לקבל ערך של 4 יח'\שעה ולהגדיר את המכפלה כ-4.

לא ניתן לבחור כל ערך: מטעמי בטיחות, קיימת 'מגבלה קשיחה', שתלויה בגיל המטופל. 'המגבלה הקשיחה' של maxIOB נמוכה יותר ב-AMA מאשר ב-SMB. עבור ילדים, הערך הוא הנמוך ביותר ואילו עבור מבוגרים עם תנגודת גבוהה לאינסולין, הוא הגדול ביותר.

הפרמטרים הקשיחים ב-AndroidAPS הם:

* ילד\ה: 2
* מתבגר\ת: 5
* מבוגר\ת: 10
* מבוגר\ת עם תנגודת אינסולין גבוהה: 12
* הריון: 25

*See also [overview of hard-coded limits](#overview-of-hard-coded-limits).*

### מינון אינסולין פעיל מרבי ממנו OpenAPS לא יחרוג (maxIOB)

פרמטר זה מגביל את האינסולין הבזאלי הפעיל המרבי שבו AndroidAPS עדיין עובד. אם האינסולין הפעיל גבוה יותר, הלופ מפסיק להזריק אינסולין בזאלי נוסף עד שהאינסולין הפעיל נמצא מתחת למגבלה.

ערך ברירת המחדל הוא 2, אבל עליכם להגדיל את ערך זה לאט כדי לראות כמה הוא משפיע עליכם ואיזה ערך מתאים ביותר. ה-Max-IOB שונה עבור כל אחד ותלוי גם במינון היומי הממוצע (TDD). מטעמי בטיחות, קיימת מגבלה שתלויה בקטגוריית גיל המטופל. 'המגבלה הקשיחה' של maxIOB נמוכה יותר ב-AMA מאשר ב-SMB.

* ילד\ה: 3
* מתבגר\ת: 5
* מבוגר\ת: 7
* מבוגר\ת עם תנגודת אינסולין גבוהה: 12
* הריון: 25

*See also [overview of hard-coded limits](#overview-of-hard-coded-limits).*

### הפעלת חישוב רגישות אוטומטי (Autosens)

Here, you can chose, if you want to use the [sensitivity detection](../DailyLifeWithAaps/SensitivityDetectionAndCob.md) autosens or not.

### וויסות ערכי מטרה ע"י Autosens

אם אפשרות זו מופעלת, autosens יכול להתאים גם ערכי מטרה (לצד הבזאלי וה-ISF). זה מאפשר ל-AndroidAPS לעבוד באופן יותר 'אגרסיבי' או להיפך. ערך המטרה האמיתי עשוי להיות מושג מהר יותר תוך שימוש ב-Autosens.

### הגדרות מתקדמות

**השתמש בדלתא ממוצעת קצרה במקום בנתונים פשוטים** אם תפעילו תכונה זו, AndroidAPS ישתמש בממוצע הקצר של דלתא/גלוקוז בדם מ-15 הדקות האחרונות, שהוא בדרך כלל הממוצע של שלושת הערכים האחרונים. זה עוזר ל-AndroidAPS לעבוד עם יותר יציבות כשמקורות הנתונים רועשים כמו לדוגמה xDrip+ עם Libre.

**מכפלת בטיחות בזאלי יומי מרבי/0> היא מגבלת בטיחות חשובה. הגדרת ברירת המחדל (שלא סביר שתצטרכו לשנות) היא 3. המשמעות היא ש-AndroidAPS לעולם לא יורשה להגדיר מינון בזאלי זמני שהוא יותר מפי 3 מהמינון הבזאלי השעתי הגבוה ביותר שתוכנת במשאבה של משתמש. דוגמה: אם המינון הבזאלי הגבוה ביותר בפרופיל הוא 1.0 יח'\שעה ומכפלת הבטיחות הוא 3, אז AndroidAPS יכול להגדיר מינון בזאלי זמני מרבי של 3.0 יח'\שעה (=3X1.0).</p> 

ערך ברירת המחדל: 3 (לא צריך לשנות אלא אם כן אתם באמת צריכים ויודעים מה אתם עושים)

**מכפלת בטיחות בזאלי נוכחי/0> היא מגבלת בטיחות חשובה. הגדרת ברירת המחדל (שלא סביר שתצטרכו לשנות) היא 4. המשמעות היא ש-AndroidAPS לעולם לא יורשה להגדיר מינון בזאלי זמני שהוא יותר מפי 3 מהמינון הבזאלי השעתי הגבוה ביותר שתוכנת במשאבה של משתמש.</p> 

ערך ברירת המחדל: 4 (לא צריך לשנות אלא אם כן אתם באמת צריכים ויודעים מה אתם עושים)

**נמנום בולוס** התכונה "נמנום בולוס" פועלת לאחר בולוס ארוחה. AAPS אינו מגדיר מינונים בזאליים זמניים נמוכים לאחר ארוחה במשך משך פעילות האינסולין חלקי הפרמטר "נודניק בולוס". ערך ברירת המחדל הוא 2. כלומר עם DIA של 5 שעות, "נודניק הבולוס" יהיה באורך של 2.5 שעות (=5/2).

ערך ברירת מחדל: 2

* * *

(Open-APS-features-overview-of-hard-coded-limits)=

## סקירה כללית של מגבלות קשיחות

<table border="1">
  
<thead>
  <tr>
    <th width="200"></th>
    <th width="75">ילד\ה</th>
    <th width="75">מתבגר\ת</th>
    <th width="75">מבוגר\ת</th>
    <th width="75">מבוגר\ת עם תנגודת אינסולין גבוהה</th>
    <th width="75">הריון</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>בולוס מקס'</td>
    <td>5.0</td>
    <td>10.0</td>
    <td>17.0</td>
    <td>25.0</td>
    <td>60.0</td>
  </tr>
  <tr>
    <td>משך פעילות אינסולין מינימלי</td>
    <td>5.0</td>
    <td>5.0</td>
    <td>5.0</td>
    <td>5.0</td>
    <td>5.0</td>
  </tr>
  <tr>
    <td>משך פעילות אינסולין מקסימלי</td>
    <td>9.0</td>
    <td>9.0</td>
    <td>9.0</td>
    <td>9.0</td>
    <td>10.0</td>
  </tr>
  <tr>
    <td>יחס פחמ' מינימלי</td>
    <td>2.0</td>
    <td>2.0</td>
    <td>2.0</td>
    <td>2.0</td>
    <td>0.3</td>
  </tr>
  <tr>
    <td>יחס פחמ' מקסימלי</td>
    <td>100.0</td>
    <td>100.0</td>
    <td>100.0</td>
    <td>100.0</td>
    <td>100.0</td>
  </tr>
  <tr>
    <td>אינסולין פעיל מקסימלי-AMA</td>
    <td>3.0</td>
    <td>5.0</td>
    <td>7.0</td>
    <td>12.0</td>
    <td>25.0</td>
  </tr>
  <tr>
    <td>אינסולין פעיל מקסימלי-SMB</td>
    <td>7.0</td>
    <td>13.0</td>
    <td>22.0</td>
    <td>30.0</td>
    <td>70.0</td>
  </tr>
  <tr>
    <td>בזאלי מקסימלי</td>
    <td>2.0</td>
    <td>5.0</td>
    <td>10.0</td>
    <td>12.0</td>
    <td>25.0</td>
  </tr>
</tbody>
</table>