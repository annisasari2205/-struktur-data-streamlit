import streamlit as st
from collections import deque

# =========================
# KELAS ANTRIAN BANK
# =========================
class BankQueue:
    def __init__(self):
        self.regular_queue = deque()
        self.vip_queue = deque()
        self.counter = 0

    def take_number(self, name, is_vip=False):
        self.counter += 1

        customer = {
            "number": self.counter,
            "name": name
        }

        if is_vip:
            self.vip_queue.append(customer)
            return f"VIP-{self.counter}"
        else:
            self.regular_queue.append(customer)
            return str(self.counter)

    def call_next(self):
        if self.vip_queue:
            return self.vip_queue.popleft()

        elif self.regular_queue:
            return self.regular_queue.popleft()

        return None

    def get_queues(self):
        return list(self.vip_queue), list(self.regular_queue)


# =========================
# KONFIGURASI HALAMAN
# =========================
st.set_page_config(
    page_title="Sistem Antrian Bank",
    layout="wide"
)

st.title("🏦 Sistem Antrian Bank")

# =========================
# SESSION STATE
# =========================
if "bank" not in st.session_state:
    st.session_state.bank = BankQueue()

# =========================
# SIDEBAR
# =========================
with st.sidebar:
    st.header("📝 Ambil Nomor Antrian")

    name = st.text_input("Nama Anda")
    is_vip = st.checkbox("VIP / Lansia / Disabilitas")

    if st.button("Ambil Nomor", use_container_width=True):
        if name:
            number = st.session_state.bank.take_number(
                name,
                is_vip
            )

            st.success(
                f"Nomor antrian Anda: {number}"
            )

            st.rerun()

        else:
            st.warning("Masukkan nama Anda!")

# =========================
# TAMPILKAN ANTREAN
# =========================
vip_queue, regular_queue = (
    st.session_state.bank.get_queues()
)

col1, col2 = st.columns(2)

with col1:
    st.subheader("👑 Antrian VIP")

    if vip_queue:
        for customer in vip_queue:
            st.info(
                f"**{customer['number']}** - {customer['name']}"
            )
    else:
        st.write("Tidak ada antrian VIP")

with col2:
    st.subheader("👤 Antrian Reguler")

    if regular_queue:
        for customer in regular_queue:
            st.info(
                f"**{customer['number']}** - {customer['name']}"
            )
    else:
        st.write("Tidak ada antrian reguler")

# =========================
# PANGGIL ANTRIAN
# =========================
st.subheader("📢 Panggil Antrian")

if st.button(
    "Panggil Selanjutnya",
    use_container_width=True
):
    next_customer = (
        st.session_state.bank.call_next()
    )

    if next_customer:
        st.success(
            f"Silakan nomor {next_customer['number']} "
            f"({next_customer['name']}) "
            f"menuju ke teller!"
        )

        st.balloons()
        st.rerun()

    else:
        st.warning("Tidak ada antrian")
