# Hammer Time Player Auction Portal

A comprehensive web-based real-time player auction system built with Java Servlets, MySQL, and Vanilla JavaScript. Features separate admin and team portals for conducting live sports player auctions similar to IPL or Pro Kabaddi.

## 🚀 Features

### Admin Portal
- **Player Management**: Add, edit, and delete players with image upload
- **Live Auction Control**: Start auctions, monitor bids in real-time
- **Auction Finalization**: Complete sales and update team balances automatically
- **Results Dashboard**: View complete auction results and statistics

### Team Portal
- **Team Registration**: Secure registration with password hashing
- **Real-Time Bidding**: Place bids with instant validation and updates
- **Balance Management**: Track remaining budget in real-time
- **Player Portfolio**: View purchased players and spending summary

## 📋 Prerequisites

Before you begin, ensure you have the following installed:

1. **Java JDK 8 or higher**
2. **Apache Tomcat 9.x or 10.x**
3. **XAMPP** (for MySQL database)
4. **MySQL Connector/J 8.x** (JDBC Driver)

## 🛠️ Installation & Setup

### Step 1: Database Setup

1. Start XAMPP and ensure MySQL is running
2. Open phpMyAdmin (http://localhost/phpmyadmin)
3. Execute the SQL script located at `database/schema.sql`
   - This will create the `auction_db` database
   - Create all required tables (admin, teams, players, bids)
   - Insert sample data for testing

### Step 2: Download MySQL Connector

1. Download MySQL Connector/J from: https://dev.mysql.com/downloads/connector/j/
2. Extract the JAR file (e.g., `mysql-connector-j-8.0.33.jar`)
3. Copy it to `webapp/WEB-INF/lib/` directory

### Step 3: Download Gson Library

1. Download Gson from: https://repo1.maven.org/maven2/com/google/code/gson/gson/2.10.1/gson-2.10.1.jar
2. Copy it to `webapp/WEB-INF/lib/` directory

### Step 4: Compile Java Files

Open Command Prompt/Terminal in the project root directory:

```bash
# Create build directory
mkdir build\classes

# Compile all Java files
javac -d build\classes -cp "webapp\WEB-INF\lib\*" src\com\auction\**\*.java
```

### Step 5: Create WAR File

```bash
# Copy compiled classes to webapp
xcopy /E /I build\classes webapp\WEB-INF\classes

# Create WAR file
cd webapp
jar -cvf auction.war *
```

### Step 6: Deploy to Tomcat

1. Copy `auction.war` to Tomcat's `webapps` directory
   - Example: `C:\Program Files\Apache Software Foundation\Tomcat 9.0\webapps\`
2. Start Tomcat server
3. Wait for automatic deployment (Tomcat will extract the WAR file)

### Step 7: Access the Application

Open your browser and navigate to:
- **Main Portal**: http://localhost:8080/auction/
- **Admin Login**: http://localhost:8080/auction/admin/login.html
- **Team Login**: http://localhost:8080/auction/team/login.html

## 🔐 Default Credentials

### Admin
- **Username**: admin
- **Password**: admin123

### Sample Teams
All sample teams have the same password:
- **Username**: mumbai, chennai, or kolkata
- **Password**: admin123

## 📁 Project Structure

```
auction/
├── src/
│   └── com/auction/
│       ├── dao/              # Data Access Objects
│       ├── model/            # Entity classes
│       ├── servlet/          # Servlets (admin & team)
│       ├── util/             # Utility classes
│       └── filter/           # Authentication filters
├── webapp/
│   ├── admin/                # Admin portal pages
│   ├── team/                 # Team portal pages
│   ├── css/                  # Stylesheets
│   ├── js/                   # JavaScript files
│   ├── images/               # Uploaded images
│   ├── WEB-INF/
│   │   ├── web.xml           # Deployment descriptor
│   │   ├── lib/              # JAR dependencies
│   │   └── classes/          # Compiled classes
│   └── index.html            # Landing page
└── database/
    └── schema.sql            # Database schema
```

## 🎯 Usage Guide

### For Administrators

1. **Login** to admin portal
2. **Add Players**: Navigate to "Add Player" and fill in details
3. **Start Auction**: Go to "Live Auction", select a player, and click "Start Auction"
4. **Monitor Bids**: Watch real-time bids coming in from teams
5. **Finalize Sale**: Click "Finalize Sale" to complete the auction
6. **View Results**: Check "Results" page for complete auction summary

### For Teams

1. **Register** your team (or login with existing credentials)
2. **Browse Players**: View available players and their base prices
3. **Join Auction Room**: Wait for admin to start an auction
4. **Place Bids**: Enter bid amount and click "Place Bid"
5. **Track Purchases**: View your players in "My Players" section

## 🔧 Configuration

### Database Connection

Edit `src/com/auction/util/DBConnection.java` if you need to change database credentials:

```java
private static final String DB_URL = "jdbc:mysql://localhost:3306/auction_db";
private static final String DB_USER = "root";
private static final String DB_PASSWORD = ""; // Your MySQL password
```

### Session Timeout

Modify `webapp/WEB-INF/web.xml` to change session timeout:

```xml
<session-config>
    <session-timeout>30</session-timeout> <!-- Minutes -->
</session-config>
```

## 🐛 Troubleshooting

### Issue: "ClassNotFoundException: com.mysql.cj.jdbc.Driver"
**Solution**: Ensure MySQL Connector JAR is in `webapp/WEB-INF/lib/`

### Issue: "Connection refused" when accessing database
**Solution**: 
- Verify XAMPP MySQL is running
- Check database credentials in `DBConnection.java`
- Ensure `auction_db` database exists

### Issue: Servlets returning 404
**Solution**:
- Verify WAR file deployed correctly
- Check Tomcat logs for errors
- Ensure all servlets have `@WebServlet` annotations

### Issue: Images not uploading
**Solution**:
- Check `webapp/images/players/` directory exists
- Verify write permissions on the directory

## 🔒 Security Features

- **Password Hashing**: SHA-256 with salt
- **SQL Injection Prevention**: PreparedStatements only
- **XSS Protection**: Input sanitization
- **Session Management**: 30-minute timeout with HttpOnly cookies
- **File Upload Validation**: Image type and size restrictions

## 📊 Database Schema

### Tables
- **admin**: Admin credentials
- **teams**: Team information and balance
- **players**: Player details and auction status
- **bids**: Bid history with timestamps

### Key Relationships
- Players → Teams (sold_to_team_id)
- Bids → Players (player_id)
- Bids → Teams (team_id)

## 🎨 Technology Stack

- **Frontend**: HTML5, CSS3, Vanilla JavaScript
- **Backend**: Java Servlets
- **Database**: MySQL 8.0
- **Server**: Apache Tomcat 9.x
- **Libraries**: MySQL Connector/J, Gson

## 📝 API Endpoints

### Admin Endpoints
- `POST /admin/login` - Admin authentication
- `POST /admin/addPlayer` - Add new player
- `POST /admin/deletePlayer` - Delete player
- `POST /admin/startAuction` - Start auction for player
- `POST /admin/finalizePlayer` - Finalize auction
- `GET /admin/getPlayers` - Get all players
- `GET /admin/getAuctionStatus` - Get auction status

### Team Endpoints
- `POST /team/register` - Team registration
- `POST /team/login` - Team authentication
- `POST /team/placeBid` - Place a bid
- `GET /team/getMyPlayers` - Get team's players
- `GET /team/getBalance` - Get team balance

## 🚀 Future Enhancements

- WebSocket integration for real-time updates
- Email notifications for auction events
- Advanced analytics and reporting
- Mobile app (Android/iOS)
- Multi-season support
- Player trading system

## 📄 License

This project is created for educational purposes.

## 👥 Support

For issues or questions:
1. Check the troubleshooting section
2. Review Tomcat logs in `logs/catalina.out`
3. Verify database connections in MySQL

## 🎉 Acknowledgments

Built with modern web technologies and best practices for secure, scalable auction management.

---

**Happy Auctioning! ⚡🔨**
