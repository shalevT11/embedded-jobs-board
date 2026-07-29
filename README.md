# Embedded Jobs Board — שלו תורג'מן

לוח משרות embedded (ג'וניור + סטודנט, צפון ומרכז) שמתעדכן אוטומטית פעם ביום על ידי סוכן Claude בענן.
האתר מוגש דרך GitHub Pages מהריפו הזה.

## מבנה

- `index.html` — הדף עצמו (RTL, עברית). טוען את הנתונים מ-`data.json` בזמן ריצה. **הסוכן לא נוגע בקובץ הזה.**
- `data.json` — כל נתוני המשרות. **זה הקובץ היחיד שהסוכן היומי מעדכן.**

## חוזה עדכון לסוכן היומי (עדכון `data.json` בלבד)

1. עדכן את `lastUpdated` לתאריך הריצה (DD.MM.YYYY, שעון ישראל).
2. משרה חדשה (אין התאמה לפי company+title): הוסף עם `firstSeen` = היום, `isNew: true`, `active: true` — **רק אחרי שאימתת שהקישור שלה חי** (WebFetch).
3. משרה קיימת: השאר. אם `firstSeen` ישן מ-3 ימים — `isNew: false`.
4. בדיקת קישורים: בכל ריצה בדוק את הלינק של כל משרה עם `active: true`. עמוד 404 / ריק / "no longer available" / redirect לשגיאה ⇒ `active: false` + note בעברית "נבדק DD.MM — המשרה כבר לא באוויר". חסימה (403/login) בלבד ⇒ להשאיר פעילה עם הערת אימות ידני.
5. איכות קישורים: **`link` חייב להצביע על דף המשרה הספציפית — לעולם לא על דף חיפוש או דף קריירה כללי.** בדרושים הפורמט הוא `https://www.drushim.co.il/job/<ID>/<refcode>/` (ה-refcode מופיע ב-href של כרטיס המשרה בדף החיפוש). להעדיף דפי קריירה רשמיים של חברות על פני אגרגטורים (talentify/builtin/bandana/secrethunter — מתיישנים מהר). הקישורים העמוקים הישנים של jobs.intel.com מתים — להשתמש בפורטל intel.wd1.myworkdayjobs.com. אם אין שום קישור ישיר, מותר קישור לדף חיפוש רק עם note שמסביר מה לחפש שם.
6. **סימוני הגשה — חוק מוחלט:** השדה `applied` שייך לשלו בלבד. אסור לשנות/לאפס/למחוק אותו. משרה עם `applied: true` לעולם לא נמחקת (גם אם נסגרה — רק `active: false`). אסור לשנות `company`/`title` של רשומה קיימת (סימוני localStorage בדפדפן ממופתחים לפי company+title).

שדות: `company`, `title`, `location`, `region` (`north`|`center`|`other`), `type` (`student`|`junior`), `tags[]`, `link`, `firstSeen`, `isNew`, `active`, `note?`, `applied?` (בבעלות שלו).
