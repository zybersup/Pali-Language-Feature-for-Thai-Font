## **ข้อเสนอการพัฒนาฟีเจอร์ภาษาบาลีสำหรับฟอนต์อักษรไทย**

**Description:**

  โครงการริเริ่มเพื่อปรับปรุงการแสดงผลภาษาบาลีด้วยฟอนต์อักษรไทย โดยมุ่งเน้นการวางแนวทางให้การใช้ภาษาบาลีบนแพลตฟอร์มดิจิทัลมีความถูกต้องตามอักขรวิธี สะดวก และเป็นไปในทิศทางเดียวกัน

  An initiative to enhance Pali orthography in Thai fonts. This project provides a framework for ensuring accurate and consistent script support across digital platforms. *(English details are below.)*

### **1\. ปัญหา**

ในการเขียนภาษาบาลีด้วยอักษรไทย มีกฎทางอักขรวิธีที่สำคัญคือ ตัวอักษร **ญ (U+0E0D)** และ **ฐ (U+0E10)** จะต้องแสดงผลในรูปที่ **"ไม่มีเชิง"** และ **"ไม่มีฐาน"** เสมอ ไม่ว่าจะอยู่ในตำแหน่งใดหรือมีเครื่องหมายใดมาประสมหรือไม่ก็ตาม

**ข้อจำกัดในปัจจุบัน:**

* ฟอนต์ไทยส่วนใหญ่มักผูกรูป "ไม่มีเชิง/ไม่มีฐาน" ไว้กับกลไกการหลบหลีกเมื่อมีสระล่างหรือพินทุเท่านั้น  
* เมื่อใช้งานในบริบทภาษาบาลีแท้ๆ ที่ต้องตัดเชิง/ฐานออกทุกกรณี ผู้ใช้งานต้องเลือกเปลี่ยนรูปอักษร (glyph) ด้วยตนเอง (manual substitution) หรือใช้ Font พิเศษแยกต่างหาก  
* ฟอนต์ไทยเกือบทั้งหมดในท้องตลาด ยังไม่มีการระบุ **Language Tag** เพื่อรองรับฟีเจอร์การเปลี่ยนรูปอัตโนมัติ (localized forms) ตามมาตรฐาน OpenType

**ผลกระทบของปัญหา**

* ความผิดพลาดในการแสดงรูปอักษรที่ถูกต้อง เนื่องจากความยุ่งยากในการเลือกเปลี่ยนรูปอักษรด้วยตนเอง
* **ความถูกต้องของข้อมูล (semantic integrity):** การที่ฟอนต์ไม่รองรับ ทำให้เกิดการ "Hack" ฟอนต์หรือการใช้ตัวอักษรผิดประเภทเพื่อให้ได้ภาพที่ถูกต้อง (เช่น การนำอักขระอื่นมาตกแต่ง) ซึ่งส่งผลเสียต่อการสืบค้นข้อมูล (Search) และการประมวลผลทางภาษาในอนาคต

### **2\. ข้อเสนอการแก้ไข**

เสนอให้ใช้กลไกการรับรู้ภาษา (Language-aware rendering) แทนการสร้างรหัสอักขระใหม่ เพื่อให้ข้อมูลยังคงความเป็น Unicode มาตรฐาน:

1. **การเปลี่ยนรูปอักษร (Glyph Substitution):** เมื่อระบุรหัสภาษาเป็นบาลี (pi/pli) หรือสันสกฤต (sa/san) ให้ฟอนต์สลับรูป ญ และ ฐ เป็นแบบไม่มีเชิง/ฐานอัตโนมัติ ผ่านฟีเจอร์ **locl (Localized Forms)**  
2. **การกำหนดชุดสไตล์สำรอง (Stylistic Set):** กำหนดให้ใช้ฟีเจอร์ **ss01** เป็นมาตรฐานกลางสำหรับสลับรูปอักษรบาลี เพื่อรองรับซอฟต์แวร์ที่ไม่รองรับ Language Tag (เช่น Microsoft Word)

### **3\. ผลลัพธ์ที่คาดหวัง**

* **Web Browsers:** รองรับผ่าน HTML Tag เช่น \<span lang="pi"\>ปญฺญา\</span\> เบราว์เซอร์จะแสดงผล ญ แบบไม่มีเชิงโดยอัตโนมัติ (ทดสอบแล้วได้ผลดีใน Chrome และ Firefox)  
* **LibreOffice:** ตั้งแต่เวอร์ชัน 7.2.0 เป็นต้นไป ผู้ใช้สามารถเลือกภาษา **"Pali Thai"** ในเมนู Character Format ได้โดยตรง ซึ่งจะเรียกใช้ฟีเจอร์ในฟอนต์ตามมาตรฐานนี้ทันที  
* **Microsoft Word:** แม้จะยังไม่รองรับ Language Tag ของภาษาไทย แต่การใส่ฟีเจอร์ ss01 จะช่วยให้ผู้ใช้มีทางเลือกในการเปลี่ยนรูปอักษรให้ถูกต้องได้ผ่านเมนู Stylistic Sets
* **Desktop Apps:** ในโปรแกรมที่รองรับ (เช่น LibreOffice, Adobe InDesign) เมื่อตั้งค่าภาษาของข้อความ (language) เป็น Pali รูปอักษรจะเปลี่ยนให้ถูกต้องตามอักขรวิธีทันที  
* **Standardization:** นักพัฒนาฟอนต์ไทยสามารถใช้แนวทางนี้เป็นมาตรฐานเดียวกัน เพื่อลดความซ้ำซ้อนและเพิ่มความถูกต้องในการเผยแพร่คัมภีร์และตำราทางศาสนาในรูปแบบดิจิทัล

### **4\. ผลการทดสอบ**

จากการทดสอบเบื้องต้นในฟอนต์ต้นแบบ (ที่ได้รับการแก้ไขแล้ว) พบว่าสามารถทำงานได้สมบูรณ์ใน:

* **Browsers:** Google Chrome, Firefox, Safari (ผ่าน CSS font-feature-settings หรือ HTML lang attribute)
  ตัวอย่างการทดสอบ: https://linux.thai.net/~thep/test/palitest-ss01-woff.html
* **Office Suite:** LibreOffice (ผ่านการตั้งค่าภาษาของ Character)

### **5\. สถานะของโครงการ**

ข้อเสนอนี้เป็นความริเริ่มของชุมชนนักพัฒนาฟอนต์และผู้เชี่ยวชาญด้านภาษาไทย (Community-driven initiative) เพื่อสร้างมาตรฐานกลางในการพัฒนาฟอนต์ไทยให้รองรับการใช้งานทางศาสนาและวิชาการได้อย่างเป็นสากล

**เสนอโดย:** \[ศุภพงษ์ อารีประเสริฐกุล\]

**วัตถุประสงค์:** เพื่อปรับปรุงมาตรฐานฟอนต์ไทยให้รองรับการใช้งานที่หลากหลายและถูกต้องตามหลักภาษาศาสตร์

**อ้างอิง:** บทสนทนาของนักพัฒนาฟอนต์ไทย ที่ https://www.facebook.com/theppitak/posts/pfbid022gBrgMYTQgyvH19Yg1KLdEhYWPW1NrpT8k9gN3C1TLVVTQAQBYVftGCaRUDG1Sfkl (Theppitak Karoonboonyanan และคณะ)

## **A Proposal for Enhancing Pali Orthographic Rendering**

### **1\. The Problem**

When writing Pali in Thai script, specific orthographic rules dictate that the characters **yoYing (ญ \- U+0E0D)** and **thoThan (ฐ \- U+0E10)** must be displayed without their "base" or "descender" (the lower parts). This rule applies consistently in Pali, regardless of surrounding vowels or marks.

**Current Limitations:**

* **Conditional Removal:** Most Thai fonts only remove these bases when a subscript vowel or a phinthu (dot below) is present.  
* **Lack of Global Support:** For authentic Pali contexts, these bases must be removed globally. Currently, users have to manually switch glyphs or use specialized fonts.  
* **Missing Language Awareness:** Most Thai fonts lack the necessary metadata or logic to support automatic substitution based on the language of the text.

**Effects of the Problem**

* Errors in rendering the correct glyph forms due to the complexity of manually selecting and switching glyph variants.
* **Semantic Integrity:** Lack of font support leads to font "hacking" or using incorrect characters to achieve the desired visual result. This damages data integrity, hindering searchability and linguistic processing.

### **2\. Proposed Initiative**

This project proposes using language-aware rendering instead of creating new character codes, maintaining standard Unicode integrity:

1. **Glyph Substitution:** When the text language is set to Pali (pi/pli) or Sanskrit (sa/san), the font should automatically substitute ญ and ฐ with their baseless counterparts using the OpenType **locl (Localized Forms)** feature.  
2. **Standardized Stylistic Set:** Establishing **ss01** as a standardized Stylistic Set for Pali forms provides a consistent manual override for applications that do not support language-based features (e.g., Microsoft Word).

### **3\. Expected Behavior**

* **Web Browsers:** Using standard HTML language attributes (e.g., \<span lang="pi"\>) will automatically trigger the correct Pali forms; verified in modern browsers like Chrome and Firefox.  
* **LibreOffice:** From version 7.2.0 onwards, users can select **"Pali Thai"** in the Character Format menu, natively triggering these font features.  
* **Microsoft Word:** While Microsoft Word lacks Thai language tag support, the standardized **ss01** feature allows a consistent workaround via the Stylistic Sets menu.
* **Office/Desktop:** Selecting "Pali" as the text language in supported software (e.g., LibreOffice, InDesign) will render the characters correctly without manual intervention.
* **Standardization:** Thai font developers can adopt this approach as a common standard to reduce redundancy and improve accuracy in the digital publication of religious scriptures and texts.

### **4\. Proof of Concept**

Preliminary testing with the modified prototype font shows that it functions correctly in:

* **Browsers:** Google Chrome, Firefox, Safari (via CSS font-feature-settings หรือ HTML lang attribute)
  Test example: https://linux.thai.net/~thep/test/palitest-ss01-woff.html
* **Office Suite:** LibreOffice (via character language settings)

### **5\. Project Status**

This is a community-driven initiative by Thai font developers and experts to create a common framework for Thai fonts to support religious and academic use cases globally.

**Proposed by:** \[Supapong Areeprasertkul\]

**References:** Thai Font Developer Conversation at https://www.facebook.com/theppitak/posts/pfbid022gBrgMYTQgyvH19Yg1KLdEhYWPW1NrpT8k9gN3C1TLVVTQAQBYVftGCaRUDG1Sfkl (Theppitak Karoonboonyanan et al.)

**Status:** This is a community-driven proposal. The implementation has been tested and verified on Google Chrome, Firefox, and LibreOffice with reference-patched fonts.

## **A reference implementation of the OpenType Feature File (.fea)**

While various technical solutions can achieve this, the accompanying .fea file provides a reference implementation using OpenType features:

* **Localized Forms (locl):** For automatic substitution based on language settings.  
* **Stylistic Set (ss01):** For manual override in applications that do not support language-aware features.

    
        feature locl {
            script thai;
            
            # Pali Support
            language PAL  exclude_dflt;
                sub yoYing by yoYing.pali;
                sub thoThan by thoThan.pali;
        
            language PLI  exclude_dflt;
                sub yoYing by yoYing.pali;
                sub thoThan by thoThan.pali;
        
            # Sanskrit Support
            language SAN  exclude_dflt;
                sub yoYing by yoYing.pali;
                sub thoThan by thoThan.pali;
        } locl;
        
        feature ss01 {
            featureNames {
                name "Thai Pali/Sanskrit Forms";
            };
            sub yoYing by yoYing.pali;
            sub thoThan by thoThan.pali;
        } ss01;


## **References**

* https://www.facebook.com/theppitak/posts/pfbid022gBrgMYTQgyvH19Yg1KLdEhYWPW1NrpT8k9gN3C1TLVVTQAQBYVftGCaRUDG1Sfkl
* https://bugs.documentfoundation.org/show_bug.cgi?id=139607
* https://learn.microsoft.com/en-us/answers/questions/5091028/how-can-i-utilize-open-type-font-features-of-my-cu
* https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Fonts/OpenType_fonts_guide
* https://learn.microsoft.com/en-us/typography/script-development/thai
