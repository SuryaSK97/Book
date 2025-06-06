<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>21 Seater Bus Booking</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      padding: 20px;
    }

    h1 {
      margin-bottom: 20px;
    }

    .bus {
      display: flex;
      justify-content: center;
      gap: 40px;
      margin-bottom: 30px;
    }

    .column {
      display: grid;
      grid-template-rows: repeat(4, 70px);
      gap: 15px;
    }

    .row {
      display: flex;
      flex-direction: row;
      gap: 15px;
    }

    .back-row {
      display: flex;
      justify-content: center;
      gap: 15px;
    }

    .seat {
      width: 60px;
      height: 60px;
      background-color: #4caf50;
      border-radius: 8px;
      color: white;
      font-size: 13px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      cursor: pointer;
    }

    .seat.booked {
      background-color: #d32f2f;
      cursor: not-allowed;
    }

    .seat span {
      font-size: 11px;
    }

    #note {
      font-size: 14px;
      color: #555;
    }
  </style>
</head>
<body>
  <h1>🚌 21-Seater Bus Booking</h1>

  <div class="bus">
    <div class="column" id="left-column"></div>
    <div class="column" id="right-column"></div>
  </div>

  <div class="back-row" id="back-row"></div>

  <p id="note">Click a seat to book. Booked seats will show the passenger's name.</p>

  <script>
    const totalSeats = 21;
    const leftColumn = document.getElementById('left-column');
    const rightColumn = document.getElementById('right-column');
    const backRow = document.getElementById('back-row');

    const seats = [];

    for (let i = 1; i <= totalSeats; i++) {
      seats.push({ number: i, booked: false, name: '' });
    }

    function createSeat(seat) {
      const seatDiv = document.createElement('div');
      seatDiv.className = 'seat' + (seat.booked ? ' booked' : '');
      seatDiv.innerHTML = `<strong>${seat.number}</strong><span>${seat.booked ? seat.name : ''}</span>`;
      seatDiv.onclick = () => {
        if (!seat.booked) {
          const name = prompt(`Enter passenger name for seat ${seat.number}:`);
          if (name && name.trim() !== '') {
            seat.booked = true;
            seat.name = name.trim();
            renderSeats();
          }
        }
      };
      return seatDiv;
    }

    function renderSeats() {
      leftColumn.innerHTML = '';
      rightColumn.innerHTML = '';
      backRow.innerHTML = '';

      // Left side: seats 1-8
      for (let i = 0; i < 4; i++) {
        const row = document.createElement('div');
        row.className = 'row';
        row.appendChild(createSeat(seats[i * 2]));     // seat 1, 3, 5, 7
        row.appendChild(createSeat(seats[i * 2 + 1])); // seat 2, 4, 6, 8
        leftColumn.appendChild(row);
      }

      // Right side: seats 9-16
      for (let i = 0; i < 4; i++) {
        const row = document.createElement('div');
        row.className = 'row';
        row.appendChild(createSeat(seats[8 + i * 2]));     // seat 9, 11, 13, 15
        row.appendChild(createSeat(seats[8 + i * 2 + 1])); // seat 10, 12, 14, 16
        rightColumn.appendChild(row);
      }

      // Back row: seats 17–21
      for (let i = 16; i < 21; i++) {
        backRow.appendChild(createSeat(seats[i]));
      }
    }

    renderSeats();
  </script>
</body>
</html>
