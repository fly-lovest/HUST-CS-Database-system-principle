import java.sql.*;

public class Transform {
    static final String JDBC_DRIVER = "com.mysql.cj.jdbc.Driver";
    static final String DB_URL = "jdbc:mysql://127.0.0.1:3306/sparsedb?allowPublicKeyRetrieval=true&useUnicode=true&characterEncoding=UTF8&useSSL=false&serverTimezone=UTC";
    static final String USER = "root";
    static final String PASS = "123123";
	
    /**
     * 向sc表中插入数据
     *
     */
    public static int insertSC(Connection conn,int sno, String type, int score){
		PreparedStatement pstmt = null;
		int n = 0;
		try {
            String sql = "insert into sc values (?,?,?);";
            pstmt = conn.prepareStatement(sql);
            pstmt.setInt(1,sno);
            pstmt.setString(2,type);
            pstmt.setInt(3,score);
            n = pstmt.executeUpdate();
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        } finally {
            try {
                if (pstmt != null) {
                    pstmt.close();
                }
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        return n;
    }

    public static void main(String[] args) {
		Connection conn = null;
        Statement stmt = null;
        ResultSet rest = null;

        try {
            Class.forName(JDBC_DRIVER);
            conn = DriverManager.getConnection(DB_URL, USER, PASS);
            
			stmt = conn.createStatement();
            String sql = "select * from entrance_exam;";
            rest = stmt.executeQuery(sql);
            while(rest.next()){
                int sno = rest.getInt("sno");
				int chinese = rest.getInt("chinese");
				int math = rest.getInt("math");
				int english = rest.getInt("English");
				int physics = rest.getInt("physics");
				int chemistry = rest.getInt("chemistry");
				int biology = rest.getInt("biology");
				int history = rest.getInt("history");
				int geography = rest.getInt("geography");
				int politics = rest.getInt("politics");
				if(chinese != 0){
					insertSC(conn,sno,"chinese",chinese);
				}
				if(math != 0){
					insertSC(conn,sno,"math",math);
				}
				if(english != 0){
					insertSC(conn,sno,"english",english);
				}
				if(physics != 0){
					insertSC(conn,sno,"physics",physics);
				}
				if(chemistry != 0){
					insertSC(conn,sno,"chemistry",chemistry);
				}
				if(biology != 0){
					insertSC(conn,sno,"biology",biology);
				}
				if(history != 0){
					insertSC(conn,sno,"history",history);
				}
				if(geography != 0){
					insertSC(conn,sno,"geography",geography);
				}
				if(politics != 0){
					insertSC(conn,sno,"politics",politics);
				}
            }
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        } finally {
            try {
                if (rest != null) {
                    rest.close();
                }
                if (stmt != null) {
                    stmt.close();
                }
                if (conn != null) {
                    conn.close();
                }
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
    }
}