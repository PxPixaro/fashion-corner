# Fashion Corner वेबसाइट — इस्तेमाल करने के निर्देश

## अभी क्या तैयार है?
- Pink, gold और cream रंगों में responsive वेबसाइट।
- आपके स्क्रीनशॉट से काटी गई **54 dress images**, और उनके साथ स्क्रीनशॉट में दिखाई देने वाले दाम। ये screenshots के crops हैं; original high-resolution product photos नहीं। कुछ crops में Instagram का टेक्स्ट/price भी रह सकता है; बेहतर फोटो मिलने पर replace करें।
- हर पेज पर **30 dresses (desktop पर 5 columns × 6 rows)**; Next/Previous pagination। मोबाइल पर 2 columns।
- WhatsApp ऑर्डर, कॉल, Instagram profile link, दुकान की जानकारी।
- लोगो को **3 सेकंड दबाकर** PIN **123** डालें, तब editing interface खुलेगा।
- फोटो या MP4 upload/replace (अधिकतम **5 MB**), नाम, कीमत, offer price, stock, order, delete, add, store details edit।
- Supabase जोड़ने के बाद सभी बदलाव database में save होंगे और Realtime से खुली वेबसाइट पर दिखेंगे।

## महत्वपूर्ण: PIN 123 सुरक्षित authentication नहीं है
PIN 123 सिर्फ छिपे हुए admin interface को खोलता है। Supabase पर कोई बदलाव करने के लिए **अलग सुरक्षित Supabase email/password login और database में admin permission** अनिवार्य है। सिर्फ PIN जानकर कोई भी cloud data नहीं बदल सकता।

## Supabase जोड़ने के स्टेप्स
1. Supabase में project बनाएँ। SQL Editor में `supabase.sql` चलाएँ। अगर realtime publication की दो lines पहले से add हो चुकी हों तो उन्हें दोबारा न चलाएँ।
2. Authentication → Users में अपने लिए admin email/password user बनाएँ। SQL Editor में यह चलाएँ: `insert into public.admin_users(user_id) values ('YOUR_AUTH_USER_UUID');` UUID Supabase Authentication से कॉपी करें। **Email password 123 न रखें; मजबूत password रखें।**
3. `config.js` में project URL और **publishable / anon key** भरें। `service_role` या secret key कभी browser में न डालें।
4. `products.json` में शुरुआती 54 products हैं। Supabase के `products` table में इन्हें import करना होगा, लेकिन image paths `assets/dress-XX.webp` website hosting पर accessible होने चाहिए। सबसे आसान: वेबसाइट की `assets` folder को साथ host करें और initial products import करें। Import करने के लिए `seed_products.py` के निर्देश नीचे देखें।
5. सभी फाइलें किसी static hosting पर HTTPS के साथ deploy करें (Netlify, Vercel या अन्य)। `index.html` को `file://` से खोलने के बजाय local web server चलाएँ: `python -m http.server 8000` और browser में `http://localhost:8000` खोलें।
6. लोगो को 3 सेकंड hold → PIN `123` → Supabase admin email/password → edit। Save के बाद online database update होगा।

## Initial products Supabase में कैसे डालें?
`products.json` को JSON-to-CSV converter से CSV में बदलें; Supabase Table Editor → `products` → Import CSV। CSV में `id,name,price,offer_price,media_url,media_type,available,sort_order` columns रखें। Image paths relative हैं, इसलिए वेबसाइट hosting में `assets` directory साथ रखें। Alternatively SQL insert generate करें।

## MP4 और photo
MP4 **5 MB से कम** होना चाहिए। बेहतर performance के लिए 720p, H.264 codec, 10–20 सेकंड, compressed MP4 रखें। वीडियो autoplay नहीं होते, जिससे page हल्का रहता है। बड़ी images को WebP/JPEG में compress करें।

## अतिरिक्त जानकारी
- WhatsApp और Call: **9898221813** (आपके screenshot से)।
- Instagram: **@fashioncorner5866**।
- Location: Surat–Kamrej; prepaid orders; all India delivery।
- Offer Price खाली है, वेबसाइट पर `Offer: —` दिखता है।
- Supabase configure नहीं होने पर edit changes केवल उसी browser में localStorage में सेव होते हैं, सबके लिए live नहीं होते।
- Realtime connection काम करने के लिए Supabase publication, RLS policies और deployment सही होने चाहिए।
- `admin_users` table में केवल आपकी authorized user ID डालें।
