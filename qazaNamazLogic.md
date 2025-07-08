from datetime import datetime, timedelta

def get_qaza_namaz():
    gender = input("Enter your gender (male/female): ").strip().lower()
    dob_str = input("Enter your date of birth (YYYY-MM-DD): ").strip()
    end_date_str = input("Enter the date till you want to calculate qaza namaz (YYYY-MM-DD): ").strip()
    
    dob = datetime.strptime(dob_str, "%Y-%m-%d")
    end_date = datetime.strptime(end_date_str, "%Y-%m-%d")
    
    if gender == "male":
        baligh_age = 13  # Males become baligh at 13 years
    elif gender == "female":
        baligh_age = 11   # Females become baligh at 11 years
        period_days = int(input("On average, how many days per month do you have periods? "))
    else:
        print("Invalid gender entered.")
        return
    
    baligh_date = dob + timedelta(days=baligh_age * 365)
    
    if baligh_date > end_date:
        print("You were not baligh at the given end date, so no qaza namaz.")
        return
    
    total_days = (end_date - baligh_date).days
    
    if gender == "female":
        total_months = total_days // 28  # Approximate months
        missed_days_due_to_periods = total_months * period_days
        total_days -= missed_days_due_to_periods
    
    print(f"Total Qaza Namaz Days: {total_days}")
    
    # Ramzan namaz check
    prayed_ramzan = input("Did you pray all Namaz in Ramadan? (yes/no): ").strip().lower()
    if prayed_ramzan == "yes":
        ramzan_days = (total_days // 354) * (30 -period_days)  # Approximate number of Ramzan days in total period
        total_days -= ramzan_days
        print(f"Total Ramzan Days: {ramzan_days}")
    print(f"Total Qaza Namaz Days after Ramazan: {total_days}")

    # Jummah namaz check
    prayed_jummah = input("Did you regularly pray Jummah? (yes/no): ").strip().lower()
    if prayed_jummah == "yes":
        fridays_count = sum(1 for i in range(total_days) if (baligh_date + timedelta(days=i)).weekday() == 4)
        total_days -= fridays_count
        print(f"Total Jummah Days: {fridays_count}")
    print(f"Total Qaza Namaz Days after Jummah and Ramadan: {total_days}")
    
    
if __name__ == "__main__":
    get_qaza_namaz()
