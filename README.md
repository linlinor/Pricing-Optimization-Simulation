import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

def simulate_demand(price, intercept=150, slope=2.5):
    return np.maximum(0, intercept - slope * price)

def simulate_profit(prices, cost_per_unit):
    units = simulate_demand(prices)
    revenue = prices * units
    cost = units * cost_per_unit
    profit = revenue - cost
    return pd.DataFrame({
        "Price": prices,
        "UnitsSold": units,
        "Revenue": revenue,
        "Profit": profit
    })

def plot_results(df):
    fig, ax = plt.subplots(figsize=(10, 6))
    ax.plot(df["Price"], df["Revenue"], label="Revenue", linewidth=2)
    ax.plot(df["Price"], df["Profit"], label="Profit", linewidth=2)
    opt_idx = df["Profit"].idxmax()
    ax.axvline(df.loc[opt_idx, "Price"], color="red", linestyle="--", label="Optimal Price")
    ax.set_xlabel("Price")
    ax.set_ylabel("USD")
    ax.set_title("Revenue & Profit vs. Price")
    ax.grid(True)
    ax.legend()
    plt.tight_layout()
    plt.show()

if __name__ == "__main__":
    price_range = np.linspace(10, 60, 100)
    df_result = simulate_profit(price_range, cost_per_unit=10)
    plot_results(df_result)
