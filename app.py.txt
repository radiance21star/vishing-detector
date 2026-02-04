import streamlit as st
import re

st.set_page_config(page_title="Vishing Detector", layout="wide")
st.title("🛡️ Real-Time Vishing Scam Detector")
st.markdown("**India Banking/Insurance | Vulnerable Client Protection**")

# SIMPLEST scam detection - NO APIs needed
SCAM_WORDS = ['otp', 'urgent', 'account suspend', 'send money', 'claim frozen', 'kyc update', 'block card']

st.subheader("📝 Paste Call Transcript or Type Live Speech")
user_input = st.text_area("Enter what you hear on the call:", height=150, 
                         placeholder="Your account is suspended. Send OTP immediately for verification...")

if st.button("🔍 DETECT SCAM", type="primary"):
    if user_input:
        text = user_input.lower()
        score = 0
        risks = []
        
        # Check each scam word
        for word in SCAM_WORDS:
            if word in text:
                score += 25
                risks.append(f"🚨 Found: '{word}'")
        
        # Show result
        col1, col2 = st.columns(2)
        with col1:
            if score > 50:
                st.error(f"**🚨 SCAM DETECTED** ({score}%)")
            elif score > 25:
                st.warning(f"**⚠️ HIGH RISK** ({score}%)")
            else:
                st.success("✅ **SAFE CALL**")
        
        with col2:
            st.metric("Risk Score", f"{score}%")
        
        # Show risks found
        if risks:
            st.subheader("Detected Threats:")
            for risk in risks:
                st.write(risk)
    else:
        st.warning("Please enter some text first!")

# Demo examples
st.subheader("🧪 Test with Examples")
col1, col2, col3 = st.columns(3)
with col1:
    if st.button("Test SCAM Call"):
        st.text_area("", "Your account suspended! Send OTP now or police will come!", key="scam")
with col2:
    if st.button("Test Safe Call"): 
        st.text_area("", "This is HDFC Bank calling about your insurance renewal", key="safe")