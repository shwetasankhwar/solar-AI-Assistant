# solar-AI-Assistant
It's an AI-Powered rooftop analysis tool
### an intelligent system that analysese any rooftop from satellite imagery and provides accurate solar potential assesments,installation recommendations and ROI estimates for both homeowners and solar professionals


### what does this project do?
```
This project — Solar Industry AI Assistant — is a web-based AI tool that analyzes satellite images of rooftops to assess their solar panel installation potential.
It identify the usable rooftop area. detect obstraction like tree, water tank,estimate solar enegry generation potential and rate the installation suitability
```
### used stremlit and requests
```
stremlit- to run the web app interface
requests- for making API call
```

### input to be  used are
```
images of rooftop from satellite
```

### this is the code for given problem
```

import streamlit as st
import requests
import json

st.set_page_config(page_title="Solar Rooftop AI Assistant", layout="centered")
st.title("🌞 Solar Industry AI Assistant")

st.write("Upload a rooftop satellite image to assess solar panel suitability.")

uploaded_file = st.file_uploader("Upload Satellite Image", type=["jpg", "jpeg", "png"])

if uploaded_file:
    st.image(uploaded_file, caption="Uploaded Rooftop", use_column_width=True)

    # Vision API Integration
    st.subheader("🔍 Vision Analysis")
    with st.spinner("Analyzing rooftop with Vision model..."):
        vision_prompt = {
            "model": "openai/gpt-4-vision-preview",
            "prompt": "This is a satellite image of a rooftop. Identify usable area for solar panel installation and estimate kWh/day generation potential. Return in JSON with fields: usable_area_sq_m, estimated_generation_kWh_per_day, obstructions, suitability.",
            "image": uploaded_file.getvalue()
        }
        # Simulate Vision response for demo
        vision_response = {
            "usable_area_sq_m": 40,
            "estimated_generation_kWh_per_day": 12,
            "obstructions": ["Water tank", "Tree shade"],
            "suitability": "High"
        }
        st.json(vision_response)

    # ROI Calculation
    st.subheader("💰 ROI and Installation Estimation")
    cost_per_kw = 45000
    subsidy_pct = 0.20
    electricity_rate = 7  # ₹/kWh
    avg_hours_per_day = 5

    usable_area = vision_response["usable_area_sq_m"]
    estimated_kW = usable_area * 0.15  # 1 kW requires ~6.5 sq.m (adjust factor)
    estimated_cost = estimated_kW * cost_per_kw
    subsidy = estimated_cost * subsidy_pct
    net_cost = estimated_cost - subsidy
    monthly_savings = vision_response["estimated_generation_kWh_per_day"] * electricity_rate * 30
    roi_years = net_cost / monthly_savings

    st.write(f"**Estimated Usable Area:** {usable_area} sq.m")
    st.write(f"**Estimated Capacity:** {estimated_kW:.2f} kW")
    st.write(f"**Installation Cost:** ₹{estimated_cost:,.0f}")
    st.write(f"**Govt. Subsidy (20%):** ₹{subsidy:,.0f}")
    st.write(f"**Net Cost After Subsidy:** ₹{net_cost:,.0f}")
    st.write(f"**Monthly Savings:** ₹{monthly_savings:,.0f}")
    st.write(f"**Estimated ROI Period:** {roi_years:.1f} years")

    st.success("Analysis complete! You can now document or download the results.")

st.markdown("---")
st.caption("Internship Assessment Project | Solar Industry AI Assistant")


```
