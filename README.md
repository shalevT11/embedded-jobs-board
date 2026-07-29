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
5. איכות קישורים: להעדיף דפי קריירה רשמיים של חברות. אגרגטורים (talentify/builtin/bandana/secrethunter) מתיישנים מהר — להימנע כשיש חלופה. הקישורים העמוקים הישנים של jobs.intel.com מתים — להשתמש בפורטל intel.wd1.myworkdayjobs.com.
6. **סימוני הגשה — חוק מוחלט:** השדה `applied` שייך לשלו בלבד. אסור לשנות/לאפס/למחוק אותו. משרה עם `applied: true` לעולם לא נמחקת (גם אם נסגרה — רק `active: false`). אסור לשנות `company`/`title` של רשומה קיימת (סימוני localStorage בדפדפן ממופתחים לפי company+title).

שדות: `company`, `title`, `location`, `region` (`north`|`center`|`other`), `type` (`student`|`junior`), `tags[]`, `link`, `firstSeen`, `isNew`, `active`, `note?`, `applied?` (בבעלות שלו).
