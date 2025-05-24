---
layout: default
title: Hall of Fame
permalink: /hof/
---

<style>
body {
  background-color: #1a1a1a !important;
}
.hof-container {
  max-width: 900px;
  margin: 2rem auto;
  padding: 2rem;
  background-color: #1f1f1f;
  border-radius: 12px;
  box-shadow: 0 0 20px rgba(0,0,0,0.5);
  font-family: 'Segoe UI', sans-serif;
  color: #e0e0e0;
  line-height: 1.7;
}
.hof-container h1 {
  color: #b5f5a4;
  border-bottom: 1px solid #333;
  padding-bottom: 0.3rem;
}
.hof-container table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 1rem;
}
.hof-container th, .hof-container td {
  padding: 0.75rem 1rem;
  text-align: left;
}
.hof-container th {
  color: #a0ffa0;
  font-weight: 600;
  border-bottom: 1px solid #333;
}
.hof-container td {
  color: #e0e0e0;
}
.hof-container tr:nth-child(even) {
  background-color: #292929;
}
</style>

<div class="hof-container">
  <h1>🏆 Hall of Fame</h1>

  <p>This page acknowledges researchers who have responsibly disclosed vulnerabilities.</p>

  <table>
    <thead>
      <tr>
        <th>Reporter</th>
        <th>Severity</th>
        <th>Impact</th>
        <th>Date</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>@example_user</td>
        <td>High</td>
        <td>Account takeover via GitHub repo misconfig</td>
        <td>2025-05-23</td>
      </tr>
      <tr>
        <td>Jane Doe</td>
        <td>Critical</td>
        <td>Leaked AWS credentials on asset X</td>
        <td>2025-05-18</td>
      </tr>
    </tbody>
  </table>
</div>
