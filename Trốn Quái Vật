import streamlit as st
import random

# Cấu hình trang Streamlit
st.set_page_config(
    page_title="Monster Escape Game",
    page_icon="👹",
    layout="centered"
)

st.title("👹 Trò Chơi Trốn Chạy Khỏi Quái Vật")
st.caption("Hãy di chuyển nhân vật (🧍) đến Cửa thoát hiểm (🚪) trước khi bị Quái vật (👹) bắt!")

# Kích thước bàn cờ (5x5)
GRID_SIZE = 5

# Khởi tạo trạng thái trò chơi (Session State)
if "player" not in st.session_state:
    st.session_state.player = [0, 0]
if "monster" not in st.session_state:
    st.session_state.monster = [4, 4]
if "exit" not in st.session_state:
    st.session_state.exit = [4, 0]
if "game_over" not in st.session_state:
    st.session_state.game_over = False
if "won" not in st.session_state:
    st.session_state.won = False

# Hàm đặt lại trò chơi (Chơi lại)
def reset_game():
    st.session_state.player = [0, 0]
    st.session_state.monster = [4, 4]
    st.session_state.game_over = False
    st.session_state.won = False

# Hàm điều khiển AI của quái vật (di chuyển đuổi theo người chơi)
def move_monster():
    p_r, p_c = st.session_state.player
    m_r, m_c = st.session_state.monster

    possible_moves = []
    if m_r < p_r: 
        possible_moves.append((m_r + 1, m_c))
    elif m_r > p_r: 
        possible_moves.append((m_r - 1, m_c))

    if m_c < p_c: 
        possible_moves.append((m_r, m_c + 1))
    elif m_c > p_c: 
        possible_moves.append((m_r, m_c - 1))

    if possible_moves:
        st.session_state.monster = list(random.choice(possible_moves))

# Hàm xử lý di chuyển của người chơi
def move_player(dr, dc):
    if st.session_state.game_over or st.session_state.won:
        return

    new_r = st.session_state.player[0] + dr
    new_c = st.session_state.player[1] + dc

    # Kiểm tra xem người chơi có đi ra ngoài biên bản đồ không
    if 0 <= new_r < GRID_SIZE and 0 <= new_c < GRID_SIZE:
        st.session_state.player = [new_r, new_c]

        # Kiểm tra điều kiện thắng (đến ô cửa)
        if st.session_state.player == st.session_state.exit:
            st.session_state.won = True
            return

        # Quái vật di chuyển sau mỗi bước đi của người chơi
        move_monster()

        # Kiểm tra điều kiện thua (quái vật bắt được người chơi)
        if st.session_state.player == st.session_state.monster:
            st.session_state.game_over = True

# Hiển thị thông báo kết quả trò chơi
if st.session_state.won:
    st.balloons()
    st.success("🎉 CHÚC MỪNG! Bạn đã thoát khỏi quái vật an toàn!")
elif st.session_state.game_over:
    st.error("👹 BẠN ĐÃ THUA! Quái vật đã bắt được bạn.")

# Giao diện điều khiển (Bảng các nút bấm hướng đi)
st.write("### 🎮 Bảng Điều Khiển")
c1, c2, c3 = st.columns([1, 1, 1])
with c2:
    st.button("⬆️ Lên", on_click=move_player, args=(-1, 0), use_container_width=True)

c1, c2, c3 = st.columns([1, 1, 1])
with c1:
    st.button("⬅️ Trái", on_click=move_player, args=(0, -1), use_container_width=True)
with c2:
    st.button("🔄 Chơi lại", on_click=reset_game, use_container_width=True)
with c3:
    st.button("➡️ Phải", on_click=move_player, args=(0, 1), use_container_width=True)

c1, c2, c3 = st.columns([1, 1, 1])
with c2:
    st.button("⬇️ Xuống", on_click=move_player, args=(1, 0), use_container_width=True)

# Vẽ bản đồ trò chơi bằng lưới emoji
st.write("---")
st.write("### 🗺️ Bản Đồ Trò Chơi")
for r in range(GRID_SIZE):
    row_str = ""
    for c in range(GRID_SIZE):
        if [r, c] == st.session_state.player:
            row_str += " 🧍 "  # Người chơi
        elif [r, c] == st.session_state.monster:
            row_str += " 👹 "  # Quái vật
        elif [r, c] == st.session_state.exit:
            row_str += " 🚪 "  # Cửa thoát hiểm
        else:
            row_str += " 🟩 "  # Đường đi trống
    st.markdown(f"### `{row_str}`")
