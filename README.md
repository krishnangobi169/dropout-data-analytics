"""Student Dropout Risk 2026 - Computation, Analytics & Visualization"""
import pandas as pd
import matplotlib
matplotlib.use("Agg")
import matplotlib.pyplot as plt

df = pd.read_csv("data/student_dropout_risk_2026.csv")

# ---------- COMPUTATION ----------
print("Shape:", df.shape)
print("Missing values:", df.isna().sum().sum())
print("\nOverall dropout rate: %.2f%%" % (df["dropout"].mean() * 100))

print("\n--- Descriptive statistics (key columns) ---")
cols = ["age", "gpa_semester_1", "gpa_semester_2", "attendance_rate",
        "assignments_submitted_rate", "stress_level", "sleep_hours"]
print(df[cols].agg(["mean", "median", "std", "min", "max"]).round(2))

rate_course = (df.groupby("course_type")["dropout"].mean() * 100).sort_values(ascending=False)
rate_income = df.groupby("family_income_level")["dropout"].mean() * 100
rate_sem = df.groupby("semester")["dropout"].mean() * 100
rate_fees = df.groupby("tuition_up_to_date")["dropout"].mean() * 100
rate_sch = df.groupby("scholarship_holder")["dropout"].mean() * 100
print("\nDropout % by course:\n", rate_course.round(2))
print("\nDropout % by income:\n", rate_income.round(2))
print("\nDropout % by semester:\n", rate_sem.round(2))
print("\nDropout % by tuition paid (0=no,1=yes):\n", rate_fees.round(2))
print("\nDropout % by scholarship (0=no,1=yes):\n", rate_sch.round(2))

corr = df.select_dtypes("number").corr()["dropout"].drop("dropout").sort_values()
print("\nCorrelation of each numeric column with dropout:\n", corr.round(3))

# ---------- VISUALIZATION ----------
plt.rcParams.update({"figure.figsize": (8, 5), "axes.grid": True, "grid.alpha": .3})

# 1. BAR - dropout rate by family income level
order = ["Low", "Middle", "High"]
rate_inc = rate_income.reindex(order)
ax = rate_inc.plot(kind="bar", color=["#C44E52", "#DD8452", "#55A868"], edgecolor="black")
ax.set_title("Dropout Rate by Family Income Level"); ax.set_xlabel("Family income level"); ax.set_ylabel("Dropout rate (%)")
for i, v in enumerate(rate_inc): ax.text(i, v + .3, f"{v:.1f}", ha="center")
plt.xticks(rotation=0); plt.tight_layout(); plt.savefig("charts/bar_dropout_by_income.png", dpi=150); plt.close()

# 2. LINE - dropout rate by semester
ax = rate_sem.plot(kind="line", marker="o", color="#C44E52")
ax.set_title("Dropout Rate by Semester"); ax.set_xlabel("Semester"); ax.set_ylabel("Dropout rate (%)")
plt.tight_layout(); plt.savefig("charts/line_dropout_by_semester.png", dpi=150); plt.close()

# 3. PIE - dropout vs retained
counts = df["dropout"].value_counts().sort_index()
plt.pie(counts, labels=["Retained", "Dropout"], autopct="%1.1f%%",
        colors=["#55A868", "#C44E52"], startangle=90, wedgeprops={"edgecolor": "white"})
plt.title("Dropout vs Retained Students"); plt.tight_layout()
plt.savefig("charts/pie_dropout_share.png", dpi=150); plt.close()

# 4. HISTOGRAM - attendance rate, split by outcome
plt.hist(df.loc[df.dropout == 0, "attendance_rate"], bins=25, alpha=.6, label="Retained", color="#55A868", edgecolor="black")
plt.hist(df.loc[df.dropout == 1, "attendance_rate"], bins=25, alpha=.6, label="Dropout", color="#C44E52", edgecolor="black")
plt.title("Distribution of Attendance Rate"); plt.xlabel("Attendance rate (%)"); plt.ylabel("Number of students")
plt.legend(); plt.tight_layout(); plt.savefig("charts/hist_attendance.png", dpi=150); plt.close()

print("\nCharts saved in /charts")"""Student Dropout Risk 2026 - Computation, Analytics & Visualization"""
import pandas as pd
import matplotlib
matplotlib.use("Agg")
import matplotlib.pyplot as plt

df = pd.read_csv("data/student_dropout_risk_2026.csv")

# ---------- COMPUTATION ----------
print("Shape:", df.shape)
print("Missing values:", df.isna().sum().sum())
print("\nOverall dropout rate: %.2f%%" % (df["dropout"].mean() * 100))

print("\n--- Descriptive statistics (key columns) ---")
cols = ["age", "gpa_semester_1", "gpa_semester_2", "attendance_rate",
        "assignments_submitted_rate", "stress_level", "sleep_hours"]
print(df[cols].agg(["mean", "median", "std", "min", "max"]).round(2))

rate_course = (df.groupby("course_type")["dropout"].mean() * 100).sort_values(ascending=False)
rate_income = df.groupby("family_income_level")["dropout"].mean() * 100
rate_sem = df.groupby("semester")["dropout"].mean() * 100
rate_fees = df.groupby("tuition_up_to_date")["dropout"].mean() * 100
rate_sch = df.groupby("scholarship_holder")["dropout"].mean() * 100
print("\nDropout % by course:\n", rate_course.round(2))
print("\nDropout % by income:\n", rate_income.round(2))
print("\nDropout % by semester:\n", rate_sem.round(2))
print("\nDropout % by tuition paid (0=no,1=yes):\n", rate_fees.round(2))
print("\nDropout % by scholarship (0=no,1=yes):\n", rate_sch.round(2))

corr = df.select_dtypes("number").corr()["dropout"].drop("dropout").sort_values()
print("\nCorrelation of each numeric column with dropout:\n", corr.round(3))

# ---------- VISUALIZATION ----------
plt.rcParams.update({"figure.figsize": (8, 5), "axes.grid": True, "grid.alpha": .3})

# 1. BAR - dropout rate by family income level
order = ["Low", "Middle", "High"]
rate_inc = rate_income.reindex(order)
ax = rate_inc.plot(kind="bar", color=["#C44E52", "#DD8452", "#55A868"], edgecolor="black")
ax.set_title("Dropout Rate by Family Income Level"); ax.set_xlabel("Family income level"); ax.set_ylabel("Dropout rate (%)")
for i, v in enumerate(rate_inc): ax.text(i, v + .3, f"{v:.1f}", ha="center")
plt.xticks(rotation=0); plt.tight_layout(); plt.savefig("charts/bar_dropout_by_income.png", dpi=150); plt.close()

# 2. LINE - dropout rate by semester
ax = rate_sem.plot(kind="line", marker="o", color="#C44E52")
ax.set_title("Dropout Rate by Semester"); ax.set_xlabel("Semester"); ax.set_ylabel("Dropout rate (%)")
plt.tight_layout(); plt.savefig("charts/line_dropout_by_semester.png", dpi=150); plt.close()

# 3. PIE - dropout vs retained
counts = df["dropout"].value_counts().sort_index()
plt.pie(counts, labels=["Retained", "Dropout"], autopct="%1.1f%%",
        colors=["#55A868", "#C44E52"], startangle=90, wedgeprops={"edgecolor": "white"})
plt.title("Dropout vs Retained Students"); plt.tight_layout()
plt.savefig("charts/pie_dropout_share.png", dpi=150); plt.close()

# 4. HISTOGRAM - attendance rate, split by outcome
plt.hist(df.loc[df.dropout == 0, "attendance_rate"], bins=25, alpha=.6, label="Retained", color="#55A868", edgecolor="black")
plt.hist(df.loc[df.dropout == 1, "attendance_rate"], bins=25, alpha=.6, label="Dropout", color="#C44E52", edgecolor="black")
plt.title("Distribution of Attendance Rate"); plt.xlabel("Attendance rate (%)"); plt.ylabel("Number of students")
plt.legend(); plt.tight_layout(); plt.savefig("charts/hist_attendance.png", dpi=150); plt.close()

print("\nCharts saved in /charts")
