
import os
import streamlit as st
import pandas as pd
import re
import numpy as np
import matplotlib.pyplot as plt
from tabulate import tabulate
import subprocess
import openpyxl
from PIL import Image

data = pd.read_excel("DPSD- Comprehensive vaiva Quiz details project.xlsx")

ques = []
students = len(data["First name"].value_counts())
percent = (30 / 100)
to_calculate = int(students * percent)
regex = "Q\. ([1-9]|[1-9][0-9]{1,2}|1000)(.+)\.00$"
v = data.columns.ravel()
for i in v:
    match = re.match(regex, i)
    if match:
        ques.append(i)

dict_questions = {}

for ques_val in ques:
    lst = []
    s = data.loc[:students, ques_val]
    s = np.array(s.values)

    for filter_none in s:
        filter_none = str(filter_none)
        if filter_none.isdigit():
            lst.append(int(filter_none))
        else:
            lst.append(0)
    dict_questions[ques_val] = lst

Fascilation_index = []
Descrimination_index = []

for ques_no in ques:
    sum_first = sum(dict_questions[ques_no][:to_calculate])
    sum_last = sum(dict_questions[ques_no][-to_calculate:])

    fac = ((sum_first + sum_last) / (2 * to_calculate * max(dict_questions[ques_no])))
    Fascilation_index.append(fac)

    Des = ((sum_first - sum_last) / to_calculate * max(dict_questions[ques_no]))
    Descrimination_index.append(Des)

distrac_eff = []
for ques_no in ques:
    incorrect_responses = sum(1 for val in dict_questions[ques_no] if val == 0)
    total_responses = len(dict_questions[ques_no])
    if total_responses != 0:
        distrac_eff.append((incorrect_responses / total_responses) * 100)
    else:
        distrac_eff.append(0)

output_data = pd.DataFrame({
    "Questions": ques,
    "Fascilitation_index": Fascilation_index,
    "Discrimination_index": Descrimination_index,
    "Distract_Effectiveness": distrac_eff
})

# Limit Distract_Effectiveness to two decimal places
output_data['Distract_Effectiveness'] = output_data['Distract_Effectiveness'].round(2)

data = None

def main():
    st.title("Excel Data Analyzer")

    uploaded_file = st.file_uploader("Upload an Excel file", type=["xlsx", "xls"])

    if uploaded_file is not None:
        data = pd.read_excel(uploaded_file)
        st.success("Data loaded successfully!")
        analyze_data(data)
    st.markdown("Thank you for using my app! 👋")

st.header("Test Item Analysis")

def get_unique_terms(data):
    unique_terms = []

    for column in data.columns:
        unique_values = data[column].unique()
        if len(unique_values) > 1:
            unique_terms.append(column)

    return unique_terms

def analyze_data(data):
    unique_terms = get_unique_terms(data)

    if not unique_terms:
        st.warning("No unique terms found in the data.")
        return

    selected_term = st.selectbox("Select a term for analysis:", unique_terms)
    selected_value = st.selectbox(f"Select {selected_term}:", data[selected_term].unique())

    result_data = data[data[selected_term] == selected_value]
    st.subheader(f"{selected_term} - {selected_value} Analysis")

    st.write(output_data.style.format({'Distract_Effectiveness': "{:.2f}"}))

    st.dataframe(result_data)


df = pd.read_excel("DPSD- Comprehensive vaiva Quiz details project.xlsx")
top_30_percent = df['Grade/50.00'].quantile(0.7)
bottom_30_percent = df['Grade/50.00'].quantile(0.3)

# Filter the names based on the scores
top_names = df[df['Grade/50.00'] >= top_30_percent]['First name']
bottom_names = df[df['Grade/50.00'] <= bottom_30_percent]['First name']

# Display top 30% names
st.subheader("Top 30% Names:")
st.write(top_names)

# Display bottom 30% names
st.subheader("Bottom 30% Names:")
st.write(bottom_names)





img = Image.open("4.jpeg")
st.image(img, caption="Welcome to Ats app!", width=700, channels="RGB")

if __name__ == "__main__":
    main()
