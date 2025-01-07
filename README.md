# PumpStorage

import random
import pandas as pd


def production_function(efficiency, cap, water_in_reservoir, fixed_cost, days_in_month, min_price=-5, max_price=30, nightMWHPmod=.1, dayMWHPmod=20):
    monthly_revenue = 0
    daily_data = []
    
    for day in range(days_in_month):
            dayMWHP = dayMWHPmod*random.uniform(min_price, max_price)
            nightMWHP = nightMWHPmod*random.uniform(min_price, max_price)
            price_dif=dayMWHP-nightMWHP
            day_info = {
                "Day": day + 1,
                "Day Price (MWHP)": f"{dayMWHP:.2f}",
                "Night Price (MWHP)": f"{nightMWHP:.2f}",
                "Price Difference": f"{price_dif:.2f}",
                "Water in Reservoir (MW)":f"{water_in_reservoir}"
            }   
            if price_dif > 0 and water_in_reservoir> 0:
                water_used = water_in_reservoir
                day_production = water_in_reservoir*dayMWHP
                total_revenue = day_production 
                net_revenue = total_revenue-fixed_cost
                monthly_revenue += net_revenue
                water_used = water_in_reservoir
                water_in_reservoir -=water_used
                day_info["Day Action"]= "Revenue"
                day_info["Net Revenue"]= f"{net_revenue:.2f}"
            else:
                day_info["Day Action"] = "No production"
                day_info["Net Revenue"] = "N/A"
                
            if nightMWHP < dayMWHP:
                res_space = cap - water_in_reservoir
                water_pumped = res_space
                water_in_reservoir+=water_pumped
                day_info["Night Action"] = f"Pumped {water_pumped:.2f} MW into storage"
                 
            else:
                day_info["Night Action"] = "No pumping"
            day_info["Water Left in Reservoir (MW)"] = f"{water_in_reservoir:.2f}"
            daily_data.append(day_info)
        
    return monthly_revenue, daily_data
    df = pd.DataFrame(daily_data)
    print("Production and P&L Summary:")
    print(df)
    
#Add evaporation
#Add an additional period 
#Add cost of pumping 
#Have a cumulative profit function
#spit out graphs
#include seasonal variance
#include black start pricing 
#flow into a dcf 
