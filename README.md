# Restaurant Rush

* **Repository:** [https://github.com/<OWNER>/<REPO>](https://github.com/klawnot/SC2002_RestaurantRush/tree/main)

## Build and Run Instructions

### Prerequisites
* Java Development Kit (JDK) 17 or higher installed.

### 1. Compile
From the project root directory:

```sh
javac -d bin src/restaurantrush/*.java
```

### 2. Run
Execute the main application:

```sh
java -cp bin restaurantrush.Main
```

### 3. Clean Rebuild (Optional)
Reset compiled classes before recompiling:

* **macOS / Linux:**
```sh
rm -rf bin && mkdir bin
```

* **Windows (Command Prompt):**
```cmd
rmdir /s /q bin && mkdir bin
```

* **Windows (PowerShell):**
```powershell
Remove-Item -Recurse -Force bin; New-Item -ItemType Directory bin
```

---

## Stage Transitions

### Transition 1: Stage 1 → Stage 2 (Customer Behaviour Extension)
* **Compare Link:** https://github.com/klawnot/SC2002_RestaurantRush/compare/stage-1...stage-2

* **New Files:**
  * `VIPCustomer.java`: Added subclass of `Customer` with lower satisfaction decay (5 points/turn) and a 10% bill discount.
  * `CriticCustomer.java`: Added subclass of `Customer` with higher satisfaction decay (12 points/turn) and deterministic food selection (orders first menu item).

* **Existing Files Changed:**
  * `Customer.java`: Added baseline decay (8 points/turn) and bill settlement logic to support polymorphism across customer types.
  * `TurnController.java`: Updated arrival creation logic to schedule `VIPCustomer` at turn 7 and `CriticCustomer` at turn 13.
  * `RestaurantView.java`: Added display support for customer type labels (`getTypeLabel()`).

* **Deleted Files:**
  * None.

---

### Transition 2: Stage 2 → Stage 3 (New Role and Menu Policy)
* **Compare Link:** https://github.com/klawnot/SC2002_RestaurantRush/compare/stage-2...stage-3

* **New Files:**
  * `Host.java`: Added staff role dedicated to seating queued customers at unoccupied tables independently of the Waiter's action.
  * `ComboMeal.java`: Added composite subclass of `MenuItem` that bundles two items, sums prep units, and applies a 10% discount on combined base price.

* **Existing Files Changed:**
  * `Menu.java`: Registered composite `ComboMeal` instances into the default menu catalog.
  * `RestaurantState.java`: Added Happy Hour state tracking, active order query helpers (`getReadyOrders`, `getCookableOrders`), and safe cancellation handling.
  * `InputHandler.java`: Added prompt methods for Host seating choices and Happy Hour activation prompts.
  * `Customer.java` & `VIPCustomer.java`: Added polymorphic `calculateItemPrice()` to compute standard, VIP, and Happy Hour pricing without `instanceof` checks.
  * `Waiter.java`: Added `getOrderableTables()` helper; updated `takeOrder()` to delegate item pricing to `Customer.calculateItemPrice()`.
  * `Chef.java`: Added `canCook()` predicate to check order cooking eligibility.
  * `TurnController.java`: Integrated Host seating before Waiter actions, added Happy Hour prompts and turn boosts, and delegated collection filtering to staff and state objects.
  * `RestaurantView.java`: Added status displays for active Happy Hour duration and promotional notifications.

* **Deleted Files:**
  * None.
