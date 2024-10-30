document.addEventListener("DOMContentLoaded", function() {
    setTimeout(() => {
        document.getElementById("welcomeScreen").style.display = "none";
        document.getElementById("mainContent").style.display = "flex";
    }, 2000);

    const goalSet = new Set();
    const completedGoals = [];

    document.getElementById("addGoalBtn").addEventListener("click", function() {
        document.getElementById("mainContent").style.display = "none";
        document.getElementById("setGoalInterface").style.display = "flex";
    });

    document.getElementById("backBtn").addEventListener("click", function() {
        document.getElementById("setGoalInterface").style.display = "none";
        document.getElementById("mainContent").style.display = "flex";
    });

    document.getElementById("setGoalBtn").addEventListener("click", function() {
        const goal = document.getElementById("goalSelect").value;
        if (goal) {
            if (!goalSet.has(goal)) {
                goalSet.add(goal);
                alert(`${goal} Successfully set Goal`);
                addGoalToProgress(goal);
            } else {
                alert("This goal is already added.");
            }
            document.getElementById("goalSelect").value = "";
            document.getElementById("setGoalInterface").style.display = "none";
            document.getElementById("mainContent").style.display = "flex";
        } else {
            alert("Please select a goal.");
        }
    });

    document.getElementById("viewProgressBtn").addEventListener("click", function() {
        document.getElementById("mainContent").style.display = "none";
        document.getElementById("viewProgressInterface").style.display = "flex";
    });

    document.getElementById("backToMainBtn").addEventListener("click", function() {
        document.getElementById("viewProgressInterface").style.display = "none";
        document.getElementById("mainContent").style.display = "flex";
    });

    document.getElementById("recordWorkoutBtn").addEventListener("click", function() {
        document.getElementById("mainContent").style.display = "none";
        document.getElementById("recordWorkoutInterface").style.display = "flex";
        updateGoalStatus();
        displayCompletedGoals();
    });

    document.getElementById("backToMainFromWorkoutBtn").addEventListener("click", function() {
        document.getElementById("recordWorkoutInterface").style.display = "none";
        document.getElementById("mainContent").style.display = "flex";
    });

    document.getElementById("resetBtn").addEventListener("click", function() {
        completedGoals.length = 0; // Clear the array
        updateGoalStatus();
        displayCompletedGoals();
        alert("All records have been cleared!");
    });

    function addGoalToProgress(goal) {
        const goalsContainer = document.getElementById("goalsContainer");
        const goalItem = document.createElement("div");
        goalItem.classList.add("goal-item");
        goalItem.innerHTML = `
            ${goal}
            <button class="complete-btn">Complete</button>
            <button class="delete-btn">Delete</button>
        `;
        goalsContainer.appendChild(goalItem);

        goalItem.querySelector(".complete-btn").addEventListener("click", function() {
            if (!completedGoals.includes(goal)) {
                completedGoals.unshift(goal); // Add to the top of the completed goals
                goalItem.style.backgroundColor = "yellow";
                alert(`${goal} marked as complete!`);
                displayCompletedGoals();
            } else {
                alert(`${goal} has already been completed.`);
            }
        });

        goalItem.querySelector(".delete-btn").addEventListener("click", function() {
            goalItem.remove();
            goalSet.delete(goal);
            alert(`${goal} has been deleted.`);
        });
    }

    function displayCompletedGoals() {
        const completedGoalsContainer = document.getElementById("completedGoalsContainer");
        completedGoalsContainer.innerHTML = ""; // Clear the container

        completedGoals.forEach(goal => {
            const goalItem = document.createElement("div");
            goalItem.classList.add("completed-goal-item");
            goalItem.innerText = goal;
            completedGoalsContainer.appendChild(goalItem);
        });
    }

    function updateGoalStatus() {
        const goalStatus = document.getElementById("goalStatus");
        goalStatus.innerHTML = completedGoals.length > 0 ? 
            `Completed Goals: ${completedGoals.join(", ")}` : 
            "No goals recorded.";
    }
});
