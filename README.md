**PROJECT 3 — cooling_system_simulation**

Cooling System Controller Simulation
A Python simulation of simple control logic used in HVAC and cooling systems. It reflects an engineering understanding of system states, thresholds, and decision rules relevant to industrial control systems.

Features
• Monitors temperature readings
• Switches states based on thresholds
• Simulates controller response

Files
cooling_system_controller_sim.py

Example Usage: python cooling_system_controller_sim.py

cooling_system_controller_sim.py
class CoolingSystemController:
    def __init__(self, min_temp=16.0, max_temp=22.0):
        self.min_temp = min_temp
        self.max_temp = max_temp
        self.state = "IDLE"

  def evaluate_temperature(self, temperature):
        if temperature > self.max_temp:
            self.state = "COOLING_ON"
        elif temperature < self.min_temp:
            self.state = "COOLING_OFF"
        else:
            self.state = "STABLE"
        return self.state

  def simulate(self, readings):
        results = []
        for temp in readings:
            status = self.evaluate_temperature(temp)
            results.append((temp, status))
        return results

if __name__ == "__main__":
    readings = [18.2, 23.5, 21.0, 15.1, 19.5]
    controller = CoolingSystemController()
    for temp, status in controller.simulate(readings):
        print(f"Temperature {temp} -> System State: {status}")
