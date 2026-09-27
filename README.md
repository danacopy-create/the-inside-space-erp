# The Inside Space - ERP System Final Project

## Project Overview
An AI-powered ERP system built for a small Israeli business, utilizing n8n Cloud for automated workflows, Airtable as the central database, and OpenAI for autonomous agents and RAG capabilities.

## System Architecture & Workflows

### 1. Onboarding Workflow (`The Inside Space - Onboarding`)
תהליך קליטה אוטומטי ומבוקר למשתמשים חדשים בקורס. האוטומציה נקלטת מטופס הרשמה, מבצעת בדיקת כפילויות מול בסיס הנתונים ב-Airtable, ובהיעדר רשומה קודמת – יוצרת כרטיס תלמיד חדש עם סטטוס "פעיל".

```mermaid
graph TD
    A["טופס הרשמה (Form Trigger) <br> קליטת פרטי תלמיד חדש"] --> B["חיפוש בבסיס הנתונים (Airtable) <br> בדיקה האם המייל כבר קיים"]
    B --> C["תנאי בקרה (IF) <br> האם התלמיד חדש לחלוטין?"]
    C -- "כן (לא נמצא במערכת)" --> D["יצירת רשומת תלמיד (Airtable) <br> הוספה כפעיל עם סטטוס התחלתי"]
2. Daily Lessons Workflow (The Inside Space - Daily Lessons)
תהליך יומי אוטומטי שרץ בכל בוקר, שולף את רשימת התלמידים הפילים, מאתר את השיעור הבא עבור כל תלמיד מתוך בסיס הנתונים, שולח את התוכן ישירות למייל/לוואטסאפ של התלמיד, ומעדכן אוטומטית את התקדמות מספר השיעור בכרטיס התלמיד.
graph TD
    A["תזמון יומי (Schedule Trigger) <br> הפעלה כל בוקר בשעה 08:00"] --> B["חיפוש תלמידים פעילים (Airtable) <br> שליפת כל התלמידים בסטטוס פעיל"]
    B --> C["איתור תוכן השיעור (Airtable) <br> התאמת מספר השיעור האישי של התלמיד"]
    C --> D["שליחת הודעה (Gmail/Messaging) <br> שליחת תוכן השיעור והתרגיל לתלמיד"]
    D --> E["עדכון התקדמות (Airtable) <br> קידום מספר השיעור הבא בלופ של התלמיד"]
3. RAG Pipeline (RAG - Document Ingestion & Vectorization)
צינור לעיבוד ווקטורי של מסמכי הליבה של העסק. האוטומציה מורידה את מסמכי המדיניות והמוצרים מ-Google Drive, ממזגת ומטעינה אותם לזיכרון ווקטורי (Vector Store) תוך שימוש במודל Embeddings של OpenAI, כבסיס ידע חכם לסוכני ה-AI.
graph TD
    A["הפעלה ידנית (Manual Trigger) <br> יזום תהליך סנכרון מסמכים"] --> B["הורדת קבצים (Google Drive) <br> הורדת מסמכי Products ו-Policy"]
    B --> C["מיזוג נתונים (Merge) <br> איחוד זרמי המידע למבנה אחיד"]
    C --> D["אחסון ווקטורי (Vector Store) <br> הטמעת הטקסטים בעזרת OpenAI Embeddings"]
4. Moving Space AI Bot (Moving Space AI Bot)
בוט אינטראקטיבי מבוסס AI המאזין להודעות נכנסות בטלגרם, ממקד ומזקק את שאלת המשתמש למילת מפתח, שולף את תוכן השיעור הרלוונטי מ-Airtable, ומנסח באמצעות מודל שפה תשובה אדיבה, מקצועית ומדויקת התחומה אך ורק לתכני הקורס.
graph TD
    A["האזנה להודעות (Telegram Trigger) <br> קליטת שאלה נכנסת מהמשתמש"] --> B["זיקוק שאלה (OpenAI) <br> חילוץ מילת מפתח נקייה בלבד"]
    B --> C["שליפת נתונים (Airtable) <br> חיפוש תוכן השיעור התואם למפתח"]
    C --> D["המוח המרכזי (AI Agent & LLM) <br> ניסוח תשובה מבוססת תוכן הקורס בלבד"]
    D --> E["מענה לטלגרם (Telegram) <br> שליחת התשובה חזרה לתלמיד ועדכון סטטוס"]
5. Manager Agent (Manager Agent)
סוכן ניהולי חכם המוקדש לבעל העסק בטלגרם. הבוט מאמת את זהות השולח באמצעות תנאי בקרה קשיח, ומאפשר לנהל שאילתות אנליטיות וניהוליות על נתוני השיעורים והתלמידים ב-Airtable בזמן אמת.
graph TD
    A["טריגר טלגרם (Telegram Trigger) <br> קליטת פנייה מנהלית"] --> B["בדיקת הרשאות (IF) <br> אימות מזהה המשתמש מול מנהל המערכת"]
    B -- "מאושר" --> C["סוכן ניהולי (AI Agent & LLM) <br> הבנת השאלה האנליטית של המנהל"]
    C --> D["כלי שליפה (Airtable Tools) <br> גישה לטבלאות התלמידים והשיעורים"]
    D --> E["החזרת תשובה (Telegram) <br> הצגת הנתונים הניהוליים למנהל העסק בעברית"]
Repository Contents
Workflows (.json): All n8n automation workflows including the Manager Agent, Onboarding, Daily Lessons, and RAG pipeline.

Execution Proof: Includes telegram-proof.png demonstrating end-to-end bot execution and data retrieval.

System Documentation & Policies (Google Docs)
מסמך אפיון מערכת: צפה במסמך האפיון

מסמך מוצרים: צפה במסמך המוצרים

מסמך מדיניות: צפה במסמך המדיניות

Access & Links
Airtable Base (Read-Only): https://airtable.com/invite/l?inviteId=inv6xvBU50F0WBHsu&inviteToken=c81033da90be9269221055bce98fa11e89f704e95ed9d86069a9001b9161412&utm_medium=email&utm_source=product_team&utm_content=transactional-alerts

Telegram Integration: Connected via Telegram Bot API for the Manager Agent.

Execution Proof (Telegram)
