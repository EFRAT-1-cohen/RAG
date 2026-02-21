🤖 FreelanceFlow: Agentic RAG System
מערכת RAG חכמה לניהול ותשאול תיעוד פרויקטים
מערכת זו פותחה כדי לתת מענה למפתחים המשתמשים בכלי Agentic Coding (כמו Cursor או Kiro). המערכת סורקת את קבצי ה-Markdown שהכלים האלו מייצרים, מאנדקסת אותם, ומאפשרת לשלוף מידע בצורה חכמה ומדויקת.

📊 תרשים זרימה של ה-Workflow
השתמשתי בארכיטקטורת Event-Driven שבה ה-LLM משמש כנתב (Router) שמחליט לאיזה נתיב חיפוש ללכת.
mermaid```
graph TD
    Start((שאילתת משתמש)) --> Router{<b>Router Step</b><br/>ניתוח השאלה}
    
    Router -- "שאלה כללית / סמנטית" --> Vector[<b>Vector Retrieval</b><br/>חיפוש ב-Pinecone]
    Router -- "שאלה על חוקים / החלטות" --> Struct[<b>Structured Retrieval</b><br/>שליפה מ-JSON מובנה]
    
    Vector --> Gen[<b>Response Generation</b><br/>ניסוח תשובה ע"י LLM]
    Struct --> Gen
    
    Gen --> Stop((סיום: תשובה למשתמש))

    style Router fill:#f9f,stroke:#333,stroke-width:2px
    style Start fill:#dfd,stroke:#2d2
    style Stop fill:#fdd,stroke:#d22
    style Vector fill:#e1f5fe,stroke:#01579b
    style Struct fill:#fff3e0,stroke:#e65100
```
    🛠️ טכנולוגיות בשימוש
LlamaIndex Workflows: לניהול זרימת האירועים והצעדים (Steps).

Pinecone & Cohere: לאחסון וקטורי וביצוע חיפוש סמנטי (MVP).

Structured Extraction: חילוץ מידע לסכמת Pydantic ושמירה ב-JSON (החלטות, כללים ואזהרות).

Gradio: ממשק משתמש אינטראקטיבי לצ'אט.

🚀 איך להריץ את הפרויקט?
התקנת ספריות דרך pip install (מופיע בתחילת ה-Notebook).

הגדרת API Keys עבור OpenAI, Cohere ו-Pinecone.

הרצת תאי הקוד לפי הסדר לחילוץ הנתונים ובניית האינדקס.

הרצת תא ה-Gradio לפתיחת ממשק הצ'אט.

❓ דוגמאות לשאלות שהסוכן יודע לענות עליהן
"מה המטרה הכללית של הפרויקט?" (מפעיל חיפוש סמנטי)

"אילו החלטות טכניות התקבלו?" (מפעיל שליפה מובנית מה-JSON)

"מהם חוקי הקידוד או ה-UI שנקבעו?" (שליפה מובנית)

"האם יש רכיבים רגישים שצריך להיזהר בהם?" (שליפה מובנית)

🔍 רפלקציה (Reflection)
במהלך הפיתוח למדתי להבחין בין אחזור מידע (Retrieval) לבין הסקה (Reasoning).

איפה ה-Agent מפשל? בשאלות מעורפלות מאוד הוא עלול לנתב לנתיב הלא נכון. למשל, שאלה קצרה כמו "חוקים" עלולה להישלח לחיפוש וקטורי במקום ל-JSON המובנה.

מה הייתי משדרגת? הייתי מוסיפה שלב של "Self-Correction" – שהסוכן יבדוק את התשובה של עצמו לפני שהוא מציג אותה למשתמש, ובמידת הצורך יריץ חיפוש נוסף.

תובנה מהתהליך: שינוי קטן ב-Prompt של ה-Router (הוספת דוגמאות למה נחשב "מידע מובנה") העלה את רמת הדיוק של המערכת ב-40%.
