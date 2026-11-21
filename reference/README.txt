HIGH-Q MONTHLY SALES - COMPLETE BACKUP (21 Nov 2026)

IMPORTANT: Is kaam ke peeche koi alag software / source code / database / API nahi hai.
Har mahine Claude (AI) neeche diye gaye rules parh kar PDFs se Excel banata hai.
Python code har baar usi waqt likha aur chalaya jata hai - woh kahin save nahi hota.
Is liye asal "software" yahi 3 cheezein hain:

1_skill/SKILL.md
   Mukammal technical logic: PDF parsing (pdftotext, regex, column positions har
   distributor ke), name/rate matching (0.72/0.8/0.85 similarity, 0.05 rate bucket),
   Excel layout (Sales/Summary/Review sheets), formulas (Value = Units x T.P,
   growth), reconciliation (Rs 10 / Rs 100 tolerance), purane findings.

2_project_instructions/project_description.txt
   Project ki hidayaat (Roman Urdu, business rules, baqi files).

3_saved_rules/layout_rules.txt
   Baad mein add kiye gaye layout rules (A-Z sorting, decimal rate rule, Shifa/Khan House rates,
   value columns hidden).

DOBARA ISTEMAL KAISE KAREIN (agar sab kuch gum ho jaye):
1. claude.ai par naya Project banayein, project_description.txt ka text description mein paste karein.
2. SKILL.md ko skill ke tor par upload karein (Settings > Capabilities > Skills),
   ya har chat mein attach kar dein.
3. layout_rules.txt bhi attach / paste kar dein.
4. Mahine ke PDFs + March 2026 reference sheet + pichle saal ke totals bhejein.

Tools jo Claude istemal karta hai: pdftotext (poppler), Python (openpyxl), LibreOffice (recalc).
Koi API key, environment variable, config file ya database nahi.