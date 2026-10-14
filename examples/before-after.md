# Before and after examples

All examples are illustrative. Numbers, citations, dataset and place names are placeholders, not real findings. In every pair, the "after" text contains only information that is already in the "before" text.

## 1. Indonesian introduction (academic mode)

**Before:**
> Di era digital saat ini, Internet of Things (IoT) memainkan peran yang sangat krusial dalam berbagai sektor kehidupan. Protokol MQTT merupakan salah satu protokol komunikasi yang paling banyak digunakan dalam ekosistem IoT karena sifatnya yang ringan dan efisien. Namun demikian, broker MQTT tidak hanya rentan terhadap serangan, tetapi juga sering menjadi target utama serangan flooding — sebuah ancaman yang dapat melumpuhkan layanan secara keseluruhan. Lebih lanjut, berbagai penelitian menunjukkan bahwa metode deteksi konvensional belum mampu memberikan solusi yang komprehensif [3]–[5]. Oleh karena itu, penelitian ini hadir sebagai upaya untuk menggali secara mendalam pemanfaatan machine learning dalam rangka mendeteksi serangan flooding pada broker MQTT, dengan menguji algoritma Random Forest, XGBoost, dan LSTM pada dataset MQTTset [12]. Hasil penelitian ini diharapkan dapat memberikan kontribusi yang signifikan bagi perkembangan keamanan IoT di masa depan.

**After:**
> Protokol MQTT banyak dipakai pada perangkat IoT karena ringan dan efisien. Broker MQTT sering menjadi sasaran serangan flooding, yang dapat melumpuhkan seluruh layanan. Metode deteksi konvensional belum mampu menangani serangan ini secara memadai [3]–[5]. Penelitian ini menguji Random Forest, XGBoost, dan LSTM untuk mendeteksi serangan flooding pada broker MQTT dengan dataset MQTTset [12].

**Change report:** G1 opener, §14 vocabulary (*krusial, ekosistem, komprehensif, menggali*), G2, §8 dash, G4 connectors, G3 *dalam rangka*, G5 and H2-2 closing promise, §20 *merupakan salah satu*, H2-10 *pemanfaatan*. Citations [3]–[5] and [12], the three algorithms and the dataset name are unchanged. Author note: if the journal requires a contribution statement, replace the removed closing sentence with the specific result.

## 2. English abstract (academic mode)

**Before:**
> In today's rapidly evolving digital landscape, ensuring the security of IoT ecosystems has become more crucial than ever. This study delves into the intricate challenge of detecting flooding attacks on MQTT brokers, leveraging a comprehensive machine learning framework. Not only does our approach achieve remarkable performance, but it also showcases robust generalization across diverse scenarios. Extensive experiments demonstrate that the proposed model attains an accuracy of 98.7% and an F1-score of 0.97, significantly outperforming existing methods. These findings underscore the pivotal role of intelligent traffic analysis and pave the way for more resilient IoT infrastructures.

**After:**
> Flooding attacks can disrupt an MQTT broker and the IoT services that depend on it. This study applies machine learning to detect such attacks from broker traffic. On the test set, the proposed model reached 98.7% accuracy and an F1-score of 0.97, higher than the existing methods it was compared with [name the baselines]. [Evidence needed: which scenarios support the claim of generalization?]

**Change report:** §14 vocabulary, §1 *not only... but also*, H2-1 *demonstrate*, H2-3 *extensive experiments*, H3 *significantly* without a statistical test, §15 and H2-2 closing hype. The unsupported generalization claim is flagged for the author. 98.7% and 0.97 are unchanged.

## 3. Indonesian official report (formal mode)

**Before:**
> Dalam rangka meningkatkan kualitas layanan pendidikan, Dinas Pendidikan telah melaksanakan berbagai kegiatan strategis yang bersifat holistik dan berkelanjutan. Kegiatan tersebut tidak hanya melibatkan guru, tetapi juga kepala sekolah dan pengawas, sehingga diharapkan dapat memberikan dampak yang signifikan. Adapun kegiatan yang dilaksanakan meliputi pelatihan asesmen membaca bagi 120 guru SD dan pendampingan di 15 sekolah.

**After:**
> Dinas Pendidikan melatih 120 guru SD dalam asesmen membaca dan mendampingi 15 sekolah. Kegiatan ini melibatkan guru, kepala sekolah, dan pengawas.

**Change report:** §14 *holistik, berkelanjutan, signifikan*, G2, G3 *dalam rangka, adapun*, §17 *sehingga diharapkan...* rider, G9 *berbagai*. The figures 120 and 15 are unchanged. "Dalam rangka" would be kept in the *perihal* line of an official letter, where it is a convention.

## 4. Reviewer response (reply mode)

**Before:**
> Terima kasih atas masukan yang sangat berharga dan konstruktif dari Reviewer. Kami sangat mengapresiasi perhatian Reviewer terhadap detail penelitian ini. Sebagaimana yang Reviewer sampaikan, bagian metode memang belum menjelaskan pembagian data latih dan data uji secara rinci. Hal ini merupakan poin yang sangat penting. Oleh karena itu, kami telah menambahkan penjelasan mengenai pembagian data 80:20 dengan stratified sampling pada Bagian 3.2, halaman 6, baris 12–18.

**After:**
> Kami menambahkan penjelasan pembagian data latih dan uji (80:20, stratified sampling) di Bagian 3.2, halaman 6, baris 12–18.

**Change report:** §28 re-explaining the reviewer's own point, §24 repeated praise (one thank-you at the top of the letter is enough), §15 *poin yang sangat penting*, G4 *oleh karena itu*. The page and line range 12–18 is a number range, so the en dash is kept (§8).

## 5. Review-only mode

**Input:** "Cek apakah paragraf ini terdengar seperti AI, jangan ditulis ulang."
> Pada intinya, keamanan siber bukan sekadar persoalan teknologi, melainkan persoalan budaya organisasi. Sederhana, tapi berdampak.

**Output:**

| Quote | Pattern | Weight | Suggestion |
|---|---|---|---|
| "Pada intinya" | §3 saying that sounds deep | KUAT | Start with the claim itself |
| "bukan sekadar persoalan teknologi, melainkan persoalan budaya organisasi" | §1 not X but Y | KUAT | State Y directly, and name which organizational habits are meant if the source says |
| "Sederhana, tapi berdampak." | §2 one-line closer | KUAT | Cut, or replace with a concrete consequence from the source |
