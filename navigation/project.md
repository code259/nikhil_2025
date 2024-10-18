---
layout: page
title: Project Sprint 2
permalink: /project/
---

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<h1>Expense Tracker</h1>
<a href="{{site.baseurl}}/2024/10/15/lesson-summary_IPYNB_2_.html">All Blogs and Overview</a>
<form id="expense-form">
    <label for="item">Item:</label>
    <input id="item" type="text" placeholder="Enter Item:"><br>
    <label for="amnt">Amount:</label>
    <input id="amount" type="text" placeholder="Enter Amount:"><br>
    <label for="category">Category:</label>
    <input id="category" type="text" placeholder="Enter same category every time:"><br>
    <label for="description">Description:</label>
    <input id="description" type="text" placeholder="Enter Description:"><br>
</form>
<button id="btn" type="submit" onclick="addExpense()">Submit</button>
<button id="btn" onclick="clearExpenses()">Clear Expenses</button>

<h2>Expense Breakdown</h2>
<h4>Select Chart Type</h4>
<select id="chart-type" onchange="changeChartType()">
    <option value="pie">Pie</option>
    <option value="bar">Bar</option>
    <option value="doughnut">Doughnut</option>
    <option value="polarArea">Polar Area</option>
</select>

<canvas id="expenseChart" width="400" height="400"></canvas>

<h2>Expense List</h2>
<ul id="expense-list"></ul>

<style>
    .form {
        display: flex;
        flex-direction: column;
    }
    input {
        margin-bottom: 25px;
    }
</style>

<script>
    let expenses = JSON.parse(localStorage.getItem('expenses')) || [];
    var form = document.getElementById('expense-form');

    const ctx = document.getElementById('expenseChart').getContext('2d');

    let expenseChart = new Chart(ctx, {
        type: 'pie',
        data: {
            labels: [], // Expense descriptions
            datasets: [{
                label: 'Expenses',
                data: [], // Expense amounts
                backgroundColor: ['#FF6384', '#36A2EB', '#FFCE56', '#4BC0C0', '#9966FF', '#FF9F40'],
            }]
        },
        options: {
            responsive: true,
            plugins: {
                legend: {
                    position: 'top',
                },
            }
        }
    });

    window.onload = function() {
        displayExpenses();
        updateChart();
        console.log(expenses);
    }

    function clearExpenses() {
        localStorage.removeItem('expenses');
        expenses = [];
        displayExpenses();
        updateChart();
    }

    function addExpense(){
        const item = document.getElementById('item').value;
        const amount = document.getElementById('amount').value;
        const category = document.getElementById('category').value;
        const description = document.getElementById('description').value;

        const expense = {
            'item': item,
            'amount': parseFloat(amount),
            'category': category,
            'description': description,
        }

        expenses.push(expense);
        form.reset();
        localStorage.setItem('expenses', JSON.stringify(expenses));

        updateChart();
        newExpenses();
    }

    function displayExpenses() {
        const expenseList = document.getElementById('expense-list');
        expenseList.innerHTML = '';

        expenses.forEach((expense) => {
            const listItem = document.createElement('li');
            listItem.textContent = `${expense.item} - ${expense.description}: $${expense.amount.toFixed(2)}`;
            expenseList.appendChild(listItem);
        });
    }

    function newExpenses() {
        const expenseList = document.getElementById('expense-list');

        const listItem = document.createElement('li');
        listItem.textContent = `${expenses[expenses.length - 1].item} - ${expenses[expenses.length - 1].description}: $${expenses[expenses.length - 1].amount.toFixed(2)}`;
        expenseList.appendChild(listItem);
    }

    function updateChart() {
        // clear past stuff
        expenseChart.data.labels = [];
        expenseChart.data.datasets[0].data = [];

        expenses.forEach((expense) => {
            expenseChart.data.labels.push(expense.item);
            expenseChart.data.datasets[0].data.push(expense.amount);
        });

        expenseChart.update();
    }

    function changeChartType() {
        const selectedType = document.getElementById('chart-type').value;
        expenseChart.destroy();

        expenseChart = new Chart(ctx, {
            type: selectedType,
            data: {
                labels: expenses.map(exp => exp.item),
                datasets: [{
                    label: 'Expenses',
                    data: expenses.map(exp => exp.amount),
                    backgroundColor: ['#FF6384', '#36A2EB', '#FFCE56', '#4BC0C0', '#9966FF', '#FF9F40'],
                }]
            },
            options: {
                responsive: true,
                plugins: {
                    legend: {
                        position: 'top',
                    },
                }
            }
        });
    }

    function updateChart(data = expenses) {
        expenseChart.data.labels = data.map(exp => exp.item);
        expenseChart.data.datasets[0].data = data.map(exp => exp.amount);
        expenseChart.update();
    }
</script>

