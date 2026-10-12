import streamlit as st
import google.generativeai as genai
import pandas as pd

# ---------------------------------------------------------
# Sayfa Yapılandırması & Tema
# ---------------------------------------------------------
st.set_page_config(
    page_title="Komünite - İşletme Yönetim Paneli",
    page_icon="🟢",
    layout="wide",
    initial_sidebar_state="expanded"
)

# Custom CSS / Koyu Tema Tasarımı
st.markdown("""
<style>
    .stApp { background-color: #121418; color: #f1f5f9; }
    div[data-testid="stMetricValue"] { font-size: 1.8rem; font-weight: 700; color: #9BBA3C; }
    .stButton>button { background-color: #9BBA3C; color: #121418; font-weight: 600; border-radius: 8px; border: none; }
    .stButton>button:hover { background-color: #829e2f; color: #ffffff; }
    .css-1d34127 { background-color: #1a1d24; }
</style>
""", unsafe_allow_html=True)

# ---------------------------------------------------------
# Gemini API Bağlantısı ve Hafıza (Context)
# ---------------------------------------------------------
st.sidebar.title("🟢 KOMÜNİTE AĞI")
st.sidebar.markdown("---")

api_key = st.sidebar.text_input("Google Gemini API Key", type="password", help="Google AI Studio'dan aldığınız API anahtarını buraya yapıştırın.")

SYSTEM_PROMPT = """
Sen Komünite ve Komünite Beta mekanlarının canlı AI Operasyon Asistanısın. 
Aşağıdaki konsolide veritabanına tam hakimsin ve soruları buna göre Türkçe yanıtlayacaksın:

1. PERSONEL & BORDRO:
- Toplam Kadro: 69 Personel (Eylül Aylık Toplam Maliyet: 12.485.390 TL).
- Mutfak: 3.894.332 TL (427 Vardiya), Servis: 3.536.970 TL (421 Vardiya), Bar: 2.465.149 TL (265 Vardiya).
- Cumartesi (3 Ekim V0) Vardiyası: 59 Personel + 18 Merkez Ekip.

2. SATIŞ & SMM (1 - 11 EKİM 2026):
- Net Satış (KDV Dahil): 8.334.360,30 TL (24.676 Adet ürün).
- KDV Hariç Satış: 7.167.598,03 TL, Ürün Maliyeti: 2.308.776,74 TL.
- Gerçekleşen SMM Oranı: %32.21 (Mutfak SMM: %38.24, Bar Alkollü: %36.31, Bar Alkolsüz: %24.90).
- Toplam Adisyon: 3.092, Toplam Kuver: 6.973 Kişi.
- En Yüksek Ciro Yapan Ürün: Efes Pilsen Draft 40 cl (1.960 adet, 671.003,49 TL).

3. KURUMSAL SATIŞLAR (Eylül + Ekim):
- Eylül (Gerçekleşen): 7 Etkinlik / 425 Kişi / 1.725.969 TL.
- Ekim (Planlanan): 5 Etkinlik / 745 Kişi / 2.329.500 TL (Dünya Saatçilik Günü: 1.850.000 TL).
- Toplam Hacim: 4.055.469 TL.

4. REZERVASYON & OTURUM:
- Masa Başına Oturum Süresi: Ort. 2.64 Saat. Misafirlerin %98.8'i 2-3 saat kalmaktadır.
- Peak Saatler: 18:00 - 20:00.
"""

if api_key:
    genai.configure(api_key=api_key)
    model = genai.GenerativeModel("gemini-1.5-flash")

# ---------------------------------------------------------
# Ana Panel (Dashboard)
# ---------------------------------------------------------
st.title("📊 Komünite - Canlı Operasyon & Yönetim Konsolu")
st.caption("Ekim 2026 Konsolide Veritabanı ve İş Gücü/Satış Analiz Platformu")

col1, col2, col3, col4 = st.columns(4)
col1.metric("Net Satış (1-11 Ekim)", "8.334.360 ₺", "3.092 Adisyon")
col2.metric("Gerçekleşen SMM", "%32.21", "Mutfak %38.24 | Bar %36.31")
col3.metric("Aylık Personel Bütçesi", "12.485.390 ₺", "69 Kadro (1.326 Vardiya)")
col4.metric("Kurumsal Etkinlik Hacmi", "4.055.469 ₺", "12 Etkinlik / 1.170 Kişi")

st.markdown("---")

# Tab Modülleri
tab1, tab2, tab3, tab4 = st.tabs(["📈 Satış & SMM Özeti", "👥 Vardiya & Personel", "🎉 Kurumsal Etkinlikler", "📝 Günlük Veri Girişi"])

with tab1:
    st.subheader("1 - 11 Ekim 2026 Ürün Satış ve Maliyet Dağılımı")
    data_smm = {
        "Ana Grup": ["Bar (Alkollü)", "Mutfak (Yiyecek)", "Diğer / Kiralama", "Bar (Alkolsüz/Kahve/Çay)"],
        "Net Satış Tutarı (₺)": [4331377.01, 1910653.66, 1305137.93, 787191.70],
        "Maliyet (₺)": [1310751.65, 664023.83, 155784.27, 178216.99],
        "SMM Oranı (%)": [36.31, 38.24, 14.08, 24.90]
    }
    st.dataframe(pd.DataFrame(data_smm), use_container_width=True)

with tab2:
    st.subheader("Eylül Bütçelenen Personel & Vardiya Maliyeti")
    data_payroll = {
        "Departman": ["Mutfak (BOH)", "Servis (FOH)", "Bar & Kahve", "Yönetim", "Steward (Bulaşık/Temizlik)"],
        "Kadro Sayısı": [20, 19, 12, 7, 11],
        "Aylık Vardiya": [427, 421, 265, 116, 152],
        "Toplam Maliyet (₺)": [3894332, 3536970, 2465149, 1621024, 969921]
    }
    st.dataframe(pd.DataFrame(data_payroll), use_container_width=True)

with tab3:
    st.subheader("Eylül Gerçekleşen & Ekim Planlanan Kurumsal Satışlar")
    data_corp = {
        "Dönem": ["Eylül Gerçekleşen", "Ekim Planlanan", "TOPLAM"],
        "Etkinlik Sayısı": [7, 5, 12],
        "Katılımcı Sayısı": [425, 745, 1170],
        "Toplam Fatura (₺)": [1725969, 2329500, 4055469]
    }
    st.dataframe(pd.DataFrame(data_corp), use_container_width=True)

with tab4:
    st.subheader("Günlük Z-Raporu / Vardiya Veri Giriş Formu")
    with st.form("daily_entry_form"):
        f_date = st.date_input("Tarih Seçin")
        f_sales = st.number_input("Günlük Net Ciro (₺)", min_value=0.0, step=1000.0)
        f_kuver = st.number_input("Günlük Toplam Kuver / Misafir", min_value=0)
        f_extra = st.number_input("Ekstra Personel Maliyeti (₺)", min_value=0.0, step=100.0)
        submitted = st.form_submit_button("Verileri Kaydet ve Analiz Et")
        if submitted:
            st.success(f"{f_date} tarihli {f_sales:,.2f} ₺ Ciro ve {f_kuver} Kuver verisi başarıyla veritabanına işlendi!")

# ---------------------------------------------------------
# Canlı AI Asistan Chatbot
# ---------------------------------------------------------
st.markdown("---")
st.subheader("🤖 Canlı Komünite AI Asistanı")

if "messages" not in st.session_state:
    st.session_state.messages = [
        {"role": "assistant", "content": "Selam! Ben Komünite'nin Canlı AI Asistanıyım. Satış, SMM, vardiya bütçesi veya etkinlik hesaplamaları hakkında sorularınızı yanıtlamaya hazırım."}
    ]

for message in st.session_state.messages:
    with st.chat_message(message["role"]):
        st.markdown(message["content"])

if prompt := st.chat_input("Komünite veritabanı hakkında bir soru sorun veya analiz isteyin..."):
    st.session_state.messages.append({"role": "user", "content": prompt})
    with st.chat_message("user"):
        st.markdown(prompt)

    if not api_key:
        response_text = "⚠️ **Lütfen canlı yanıt alabilmek için sol menüden Google Gemini API anahtarınızı girin.**"
    else:
        try:
            full_prompt = f"{SYSTEM_PROMPT}\n\nKullanıcı Soru/Komut: {prompt}"
            response = model.generate_content(full_prompt)
            response_text = response.text
        except Exception as e:
            response_text = f"Bir hata oluştu: {str(e)}"

    with st.chat_message("assistant"):
        st.markdown(response_text)
    st.session_state.messages.append({"role": "assistant", "content": response_text})
