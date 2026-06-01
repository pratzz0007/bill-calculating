# bill-calculating
"""
Smart Grid Energy Management & Billing System
A practical application utilizing Core Python mechanics: Functions, Loops, 
Control Flow, and Loop Control Statements (Break/Continue).
"""

def calculate_base_bill(units: float) -> float:
    """
    Calculates the base electricity cost using a progressive tier control flow.
    
    Tiers:
    - Tier 1: 0 - 100 units   @ $4.50 / unit
    - Tier 2: 101 - 300 units @ $6.50 / unit
    - Tier 3: > 300 units     @ $9.00 / unit
    """
    if units <= 100:
        return units * 4.5
    elif units <= 300:
        return (100 * 4.5) + ((units - 100) * 6.5)
    else:
        return (100 * 4.5) + (200 * 6.5) + ((units - 300) * 9.0)


def apply_peak_surcharge(base_bill: float, hour: int) -> float:
    """
    Applies a dynamic 15% grid load surcharge if checked during peak hours (18:00 - 22:00).
    """
    if 18 <= hour <= 22:
        print(f"\n   [ALERT] Peak Demand Detected at {hour}:00! 15% Surcharge Applied.")
        return base_bill * 1.15
    return base_bill


def stream_appliance_data() -> float:
    """
    Simulates real-time energy telemetry aggregation from 5 household sectors.
    Demonstrates structural Loops, Exception Handling, Break, and Continue.
    """
    total_consumption = 0.0
    print("\n--- Initializing Smart Telemetry Aggregation ---")

    # Loop: Range iteration representing 5 separate appliance groups
    for sector_id in range(1, 6):
        print(f"\n[Scanning Sector {sector_id} (Appliance {sector_id})]")
        
        try:
            user_input = input("   Enter unit consumption (or '999' to Emergency Stop): ").strip()
            current_load = float(user_input)
            
            # 1. BREAK Statement: Immediate system isolation if critical flag is raised
            if current_load == 999:
                print("   [CRITICAL] Emergency System Stop sequence triggered by operator!")
                break
                
            # 2. CONTINUE Statement: Bypasses calculation steps if sector is completely idle
            if current_load <= 0:
                print(f"   [SYSTEM INFO] Sector {sector_id} reported zero or negative draw. Skipping sector computation...")
                continue
                
            # Normal Execution Block
            total_consumption += current_load
            print(f"   [SUCCESS] Registered +{current_load} units. Current Tally: {total_consumption} units.")
            
        except ValueError:
            # Exception Handling fallback to keep the program from crashing on bad inputs
            print("   [ERROR] Invalid character type detected. Skipping current entry...")
            continue

    return total_consumption


def main() -> None:
    """Main execution orchestrator."""
    print("=" * 60)
    print("     EDGE ENERGY SMART GRID & CONSUMPTION ARCHITECTURE")
    print("=" * 60)

    # Invoking metrics collection (Loops, Break, Continue)
    total_units_logged = stream_appliance_data()

    print("\n" + "=" * 25 + " METRICS & BILLING " + "=" * 25)
    print(f"Total Logged Consumption : {total_units_logged:.2f} Units")

    # Processing algorithms (Functions & Control Flow)
    base_cost = calculate_base_bill(total_units_logged)
    
    # Simulating evaluation window at 8 PM (20:00 Hours)
    simulated_hour = 20 
    final_payable_cost = apply_peak_surcharge(base_cost, simulated_hour)

    print(f"Base Subtotal Tariff     : ${base_cost:.2f}")
    print(f"Final Adjusted Balance   : ${final_payable_cost:.2f}")
    print("=" * 69)


if __name__ == "__main__":
    main()
