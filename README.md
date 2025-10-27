<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8"/>
  <title>DOCA Tours - Hotel Guest Management</title>
  <meta name="viewport" content="width=device-width, initial-scale=1"/>
  <style>
    body { background: #f6f8fa; font-family: Arial, sans-serif; margin:0; padding:20px; }
    header { background: #003366; color: #fff; padding:32px 0; text-align: center; }
    .logo {font-size: 2em; font-weight: bold;}
    .container { background:#fff; border-radius:16px; box-shadow:0 4px 28px rgba(0,0,0,0.10); padding:36px; max-width:1800px; margin:40px auto;}
    .footer {margin-top:40px; padding:22px 0; background:#003366; color:#fff; text-align:center;}
    h1 {font-size:28px;color:#1d3557;margin-bottom:18px;}
    h2 {font-size:18px;color:#1d3557;margin-bottom:12px;}
    .form-section { background:#f8f9fa; padding:20px; border-radius:12px; margin-bottom:20px;}
    .form-row { display:grid; grid-template-columns:repeat(4,1fr); gap:24px; margin-bottom:12px;}
    @media (max-width:1100px){.container{max-width:96vw;}.form-row{grid-template-columns:1fr 1fr;gap:14px;}}
    @media (max-width:768px){.form-row{grid-template-columns:1fr;}}
    label { display:block; margin-bottom:4px; color:#333; font-size:14px; font-weight:bold;}
    input,textarea { width:100%; padding:8px; border-radius:6px; border:1px solid #ccd; font-size:14px;}
    .details-row { display:grid; grid-template-columns:repeat(4,1fr); gap:16px; }
    @media (max-width:1100px){.details-row{grid-template-columns:1fr 1fr;gap:10px;}}
    @media (max-width:768px){.details-row{grid-template-columns:1fr;}}
    button {padding:10px 16px; border-radius:8px;background:#1d3557;color:#fff;font-size:14px;border:none;cursor:pointer; margin-right:8px;}
    button.save {background:#7c3aed;}
    button.delete {background:#dc3545;}
    button.sendemail {background:#228b22;}
    button.edit {background:#ff9500;}
    button.update {background:#16a34a;}
    button.cancel {background:#6c757d;}
    button.backup {background:#2563eb;}
    button:hover {opacity:0.9;}
    table { width:100%; border-collapse:collapse; margin-top:20px; font-size:15px; }
    th,td { padding:12px; border:1px solid #ddd; text-align:left; vertical-align:top;}
    th { background:#1d3557; color:#fff; font-weight:bold;}
    .status-green { background:#e8fce8; color:#259136;}
    .status-yellow { background:#fff8e1; color:#bcb415;}
    .status-red { background:#feeaea; color:#d11e1e;}
    .status-purple { background:#e6d3f7 !important; color:#7c3aed !important; }
    .guest-row.purple td:first-child { border-left-color:#7c3aed; }
    .warning-text { font-weight:bold; font-size:12px;}
    .guest-row td { border-left:4px solid transparent;}
    .guest-row.green td:first-child { border-left-color:#259136;}
    .guest-row.yellow td:first-child { border-left-color:#bcb415;}
    .guest-row.red td:first-child { border-left-color:#d11e1e;}
    small { color:#456; font-size:11px;}
    .stats { display:grid; grid-template-columns:repeat(3,1fr); gap:24px; margin:20px 0;}
    .stat-box { background:#f8f9fa; padding:16px; border-radius:8px; text-align:center;}
    .stat-number { font-size:24px; font-weight:bold; color:#1d3557;}
    .stat-label { font-size:12px; color:#666;}
    @media (max-width:1100px){table{font-size:13px;} .stats{gap:12px;} th,td{padding:8px;}}
  </style>
</head>
<body>
  <header>
    <div class="logo">DOCA Tours Hotel Guest Manager</div>
    <div style="font-size:18px; margin-top:8px;">Multi-user | Full-featured | Export & Backup</div>
  </header>
  <div class="container">
    <div class="form-section">
      <h2 id="formTitle">Add New Guest</h2>
      <div class="form-row">
        <div>
          <label>Hotel/Guest Name</label>
          <input type="text" id="hotelName" placeholder="e.g. John Smith - Suite 201" />
        </div>
        <div>
          <label>Price/Room Type</label>
          <input type="text" id="priceList" placeholder="e.g. Suite $120/night" />
        </div>
        <div>
          <label>Number of People</label>
          <input type="number" id="numPeople" min="1" placeholder="e.g. 2" />
        </div>
        <div>
          <label>Arrival Date & Time</label>
          <input type="datetime-local" id="arrivalDate" />
        </div>
      </div>
      <div>
        <label>Contacts & Details</label>
        <input type="email" id="email1" placeholder="Email (guest warning email)"/>
        <div class="details-row">
          <input type="text" id="hotelNumber" placeholder="Hotel Number"/>
          <input type="text" id="dealMaker" placeholder="Who Made This Deal"/>
          <input type="text" id="additional1" placeholder="Additional Detail"/>
          <input type="text" id="additional2" placeholder="Additional Detail"/>
        </div>
      </div>
      <br>
      <button id="addBtn" onclick="addGuest()">Add Guest</button>
      <button id="updateBtn" onclick="updateGuest()" style="display:none;background:#16a34a;">Update</button>
      <button id="cancelBtn" onclick="cancelEdit()" style="display:none;background:#6c757d;">Cancel Edit</button>
      <button onclick="clearForm()" style="background:#6c757d;">Clear Form</button>
      <button onclick="saveData()" style="background:#28a745;">Save Data</button>
      <button onclick="loadData()" style="background:#17a2b8;">Load Data</button>
      <button onclick="exportCSV()" style="background:#ffc107;color:#1d3557;">Export Data</button>
      <button onclick="backupMonth()" class="backup">Backup Month</button>
    </div>
    <div class="stats">
      <div class="stat-box"><div class="stat-number" id="totalGuests">0</div><div class="stat-label">Total Guests</div></div>
      <div class="stat-box"><div class="stat-number" id="warningCount">0</div><div class="stat-label">Need Attention</div></div>
      <div class="stat-number" id="todayArrivals">0</div><div class="stat-label">Today Arrivals</div>
    </div>
    <h2>Guest List & Warnings</h2>
    <table id="guestTable">
      <thead>
        <tr>
          <th>Guest/Hotel</th>
          <th>Room/Price</th>
          <th>People</th>
          <th>Arrival Date</th>
          <th>Email</th>
          <th>Hotel Number</th>
          <th>Deal Maker</th>
          <th>Additional 1</th>
          <th>Additional 2</th>
          <th>Days Left</th>
          <th>Status</th>
          <th>Warning</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody id="guestTableBody"></tbody>
    </table>
    <small>Monthly backup creates JSON file labeled by year-month. Export for Excel. Use by multiple people by uploading online or sharing locally.</small>
  </div>
  <div class="footer">
    &copy; 2025 DOCA Tours. All rights reserved.
  </div>
  <script>
    let guests = [];
    let guestIdCounter = 1;
    let editId = null;
    function addGuest(){
      const hotelName = document.getElementById("hotelName").value.trim();
      const priceList = document.getElementById("priceList").value.trim();
      const numPeople = document.getElementById("numPeople").value;
      const arrivalDate = document.getElementById("arrivalDate").value;
      const email1 = document.getElementById("email1").value.trim();
      const hotelNumber = document.getElementById("hotelNumber").value.trim();
      const dealMaker = document.getElementById("dealMaker").value.trim();
      const additional1 = document.getElementById("additional1").value.trim();
      const additional2 = document.getElementById("additional2").value.trim();
      if(!hotelName || !arrivalDate || !numPeople){
        alert("Please fill Hotel Name, Number of People, and Arrival Date");
        return;
      }
      const guest = {
        id: guestIdCounter++,
        hotelName, priceList, numPeople: parseInt(numPeople),
        arrivalDate, email1, hotelNumber, dealMaker, additional1, additional2,
        saved: false
      };
      guests.push(guest);
      clearForm();
      updateTable();
      updateStats();
    }
    function editGuest(id){
      const g = guests.find(guest => guest.id === id);
      if(g){
        editId = id;
        document.getElementById("hotelName").value = g.hotelName;
        document.getElementById("priceList").value = g.priceList;
        document.getElementById("numPeople").value = g.numPeople;
        document.getElementById("arrivalDate").value = g.arrivalDate;
        document.getElementById("email1").value = g.email1;
        document.getElementById("hotelNumber").value = g.hotelNumber;
        document.getElementById("dealMaker").value = g.dealMaker;
        document.getElementById("additional1").value = g.additional1;
        document.getElementById("additional2").value = g.additional2;
        document.getElementById("formTitle").textContent = "Edit Guest";
        document.getElementById("addBtn").style.display = "none";
        document.getElementById("updateBtn").style.display = "inline-block";
        document.getElementById("cancelBtn").style.display = "inline-block";
      }
    }
    function updateGuest(){
      const hotelName = document.getElementById("hotelName").value.trim();
      const priceList = document.getElementById("priceList").value.trim();
      const numPeople = document.getElementById("numPeople").value;
      const arrivalDate = document.getElementById("arrivalDate").value;
      const email1 = document.getElementById("email1").value.trim();
      const hotelNumber = document.getElementById("hotelNumber").value.trim();
      const dealMaker = document.getElementById("dealMaker").value.trim();
      const additional1 = document.getElementById("additional1").value.trim();
      const additional2 = document.getElementById("additional2").value.trim();
      if(!hotelName || !arrivalDate || !numPeople){
        alert("Please fill Hotel Name, Number of People, and Arrival Date");
        return;
      }
      const guest = guests.find(g => g.id === editId);
      if(guest){
        guest.hotelName = hotelName;
        guest.priceList = priceList;
        guest.numPeople = parseInt(numPeople);
        guest.arrivalDate = arrivalDate;
        guest.email1 = email1;
        guest.hotelNumber = hotelNumber;
        guest.dealMaker = dealMaker;
        guest.additional1 = additional1;
        guest.additional2 = additional2;
      }
      cancelEdit();
      updateTable();
      updateStats();
    }
    function cancelEdit(){
      editId = null;
      clearForm();
      document.getElementById("formTitle").textContent = "Add New Guest";
      document.getElementById("addBtn").style.display = "inline-block";
      document.getElementById("updateBtn").style.display = "none";
      document.getElementById("cancelBtn").style.display = "none";
    }
    function deleteGuest(id){
      guests = guests.filter(g => g.id !== id);
      if(editId === id) cancelEdit();
      updateTable();
      updateStats();
    }
    function saveGuest(id){
      const guest = guests.find(g => g.id === id);
      if(guest){
        guest.saved = true;
        updateTable();
        updateStats();
      }
    }
    function clearForm(){
      document.getElementById("hotelName").value = "";
      document.getElementById("priceList").value = "";
      document.getElementById("numPeople").value = "";
      document.getElementById("arrivalDate").value = "";
      document.getElementById("email1").value = "";
      document.getElementById("hotelNumber").value = "";
      document.getElementById("dealMaker").value = "";
      document.getElementById("additional1").value = "";
      document.getElementById("additional2").value = "";
    }
    function calculateStatus(arrivalDateStr){
      const arrival = new Date(arrivalDateStr);
      const now = new Date();
      const msDiff = arrival - now;
      if(msDiff < 0) return {status:"arrived", class:"red", days:-1, warning:"Already Arrived!"};
      const days = Math.floor(msDiff / 86400000);
      const hours = Math.floor((msDiff % 86400000) / 3600000);
      if(days >= 9){
        return {status:"safe", class:"green", days, warning:"Safe", daysText: days+"d "+hours+"h"};
      } else if(days === 8){
        return {status:"caution", class:"yellow", days, warning:"Send Email!", daysText: days+"d "+hours+"h"};
      } else {
        const finalWarning = (days === 0 && hours <= 5) ? "FINAL 5H WARNING!" : "Send Urgent Email!";
        return {status:"danger", class:"red", days, warning:finalWarning, daysText: days+"d "+hours+"h"};
      }
    }
    function sendReminderEmail(id) {
      const guest = guests.find(g => g.id === id);
      if (!guest || !guest.email1) {
        alert("No guest email found for this guest.");
        return;
      }
      const to = guest.email1;
      const subject = encodeURIComponent("Please don’t forget your arrival at " + guest.hotelName);
      const body = encodeURIComponent(
        `Dear Guest,\n\nThis is a friendly reminder for your upcoming stay at ${guest.hotelName}.\n\nRoom: ${guest.priceList}\nArrival Date: ${new Date(guest.arrivalDate).toLocaleString()}\nHotel Contact: ${guest.hotelNumber}\nDeal Agent: ${guest.dealMaker}\nDetails: ${guest.additional1}, ${guest.additional2}\n\nPlease don’t forget! We look forward to welcoming you.\n\nBest regards,\nHotel Team`
      );
      window.open(`mailto:${to}?subject=${subject}&body=${body}`);
    }
    function updateTable(){
      const tbody = document.getElementById("guestTableBody");
      tbody.innerHTML = "";
      const sortedGuests = [...guests].sort((a,b) => new Date(a.arrivalDate) - new Date(b.arrivalDate));
      sortedGuests.forEach(guest => {
        const statusInfo = calculateStatus(guest.arrivalDate);
        const arrivalFormatted = new Date(guest.arrivalDate).toLocaleString();
        const row = document.createElement("tr");
        row.className = "guest-row " + (guest.saved ? "purple" : statusInfo.class);
        row.innerHTML = `
          <td><strong>${guest.hotelName}</strong></td>
          <td>${guest.priceList}</td>
          <td>${guest.numPeople}</td>
          <td>${arrivalFormatted}</td>
          <td>${guest.email1}</td>
          <td>${guest.hotelNumber}</td>
          <td>${guest.dealMaker}</td>
          <td>${guest.additional1}</td>
          <td>${guest.additional2}</td>
          <td>${statusInfo.daysText}</td>
          <td class="status-${guest.saved ? "purple" : statusInfo.class}">${guest.saved ? "SAVED" : statusInfo.status.toUpperCase()}</td>
          <td class="warning-text">${guest.saved ? "You saved it!" : statusInfo.warning}</td>
          <td>
            <button class="edit" onclick="editGuest(${guest.id})">Edit</button>
            <button class="sendemail" onclick="sendReminderEmail(${guest.id})">Send Email</button>
            <button class="save" onclick="saveGuest(${guest.id})">Save</button>
            <button class="delete" onclick="deleteGuest(${guest.id})">Delete</button>
          </td>
        `;
        tbody.appendChild(row);
      });
    }
    function updateStats(){
      const total = guests.length;
      let warnings = 0;
      let todayArrivals = 0;
      const today = new Date().toDateString();
      guests.forEach(guest => {
        const statusInfo = calculateStatus(guest.arrivalDate);
        if(!guest.saved && (statusInfo.class === "yellow" || statusInfo.class === "red")) warnings++;
        if(new Date(guest.arrivalDate).toDateString() === today) todayArrivals++;
      });
      document.getElementById("totalGuests").textContent = total;
      document.getElementById("warningCount").textContent = warnings;
      document.getElementById("todayArrivals").textContent = todayArrivals;
    }
    function saveData(){
      localStorage.setItem("hotelGuests", JSON.stringify(guests));
      localStorage.setItem("hotelGuestCounter", guestIdCounter);
      alert("Data saved to browser storage!");
    }
    function loadData(){
      const savedGuests = localStorage.getItem("hotelGuests");
      const savedCounter = localStorage.getItem("hotelGuestCounter");
      if(savedGuests){
        guests = JSON.parse(savedGuests);
        guestIdCounter = savedCounter ? parseInt(savedCounter) : guests.length + 1;
        updateTable();
        updateStats();
        alert("Data loaded from browser storage!");
      } else {
        alert("No saved data found!");
      }
    }
    function backupMonth(){
      // Save as JSON with month and year in file name
      if (guests.length === 0) { alert("No guests to backup!"); return; }
      const now = new Date();
      const month = String(now.getMonth()+1).padStart(2,"0");
      const year = now.getFullYear();
      const jsonContent = JSON.stringify(guests, null, 2);
      const blob = new Blob([jsonContent], {type:'application/json'});
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = `hotel_guests_${year}-${month}.json`;
      document.body.appendChild(a);
      a.click();
      document.body.removeChild(a);
      URL.revokeObjectURL(url);
    }
    function exportCSV() {
      if (guests.length === 0) {
        alert("No guest records to export!");
        return;
      }
      const csvRows = [];
      csvRows.push([
        "Hotel/Guest Name","Room/Price","Number of People","Arrival Date & Time",
        "Email (warning)","Hotel Number","Deal Maker","Additional 1","Additional 2",
        "Days Left","Saved","Warning Status"
      ].join(','));
      guests.forEach(g => {
        const statusInfo = calculateStatus(g.arrivalDate);
        csvRows.push([
          `"${g.hotelName.replace(/"/g,'""')}"`,
          `"${g.priceList.replace(/"/g,'""')}"`,
          g.numPeople,
          `"${new Date(g.arrivalDate).toLocaleString().replace(/"/g,'""')}"`,
          `"${g.email1}"`,
          `"${g.hotelNumber}"`,
          `"${g.dealMaker}"`,
          `"${g.additional1}"`,
          `"${g.additional2}"`,
          `"${statusInfo.daysText}"`,
          `"${g.saved ? 'YES' : 'NO'}"`,
          `"${g.saved ? 'You saved it!' : statusInfo.status.toUpperCase() + ' / ' + statusInfo.warning}"`
        ].join(','));
      });
      const csvContent = csvRows.join('\r\n');
      const blob = new Blob([csvContent], {type:'text/csv'});
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = 'hotel_guest_list.csv';
      document.body.appendChild(a);
      a.click();
      document.body.removeChild(a);
      URL.revokeObjectURL(url);
    }
    setInterval(function(){ updateTable(); updateStats(); }, 60000);
    window.onload = function(){ loadData(); };
  </script>
</body>
</html>
