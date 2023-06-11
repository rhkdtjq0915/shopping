# 쇼핑 물품 관리 프로그램 만들기

### JAVA언어와 JDBS 활용
### GUI와 이벤트 활용
***

# 주요기능

판매자는 다음을 수행할 수 있습니다.
-------------
* 새 제품을 등록합니다.
* 기존 제품을 삭제합니다.
* 모든 제품을 나열합니다.
* 상품 번호로 제품을 검색합니다.
***
구매자는 다음을 수행 할 수 있습니다.
----
* 제품 확인 후 구입할 수 있습니다.
----

# 코드
**이 프로그램은 MySQL 데이터베이스를 사용하여 제품 정보를 저장합니다. 데이터베이스 연결은 생성자에서 설정됩니다.  
sellerService()메서드는 판매자 관련 작업을 모두 처리합니다.   

main()ShoppingUI 메서드는 새 개체를 만들고 주 메뉴를 표시합니다.**   
   
**먼저 참고한 프로그램 소스 입니다.**
```

```
# 수정사항
**기본적인 틀만 가져왔기 때문에 GUI를 활용하여 프로그램의 편의성을 높이고 기존 프로그램에서는 프로그램 종료 시 데이터가 보존되지 않는 불편함이 있어 데이터베이스를 활용하여 데이터가 보존될 수 있도록 수정해 보았습니다..**  

```
// 데이터베이스 연결
	  try {
          conn = DriverManager.getConnection(DB_URL, DB_USERNAME, DB_PASSWORD);
      } catch (SQLException e) {
          e.printStackTrace();
      }
	  
    frame = new JFrame("Shopping");
    frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

    panel = new JPanel();
    
    sellerButton = new JButton("판매자");
    buyerButton = new JButton("구매자");

    sellerButton.addActionListener(new ActionListener() {
      @Override
      public void actionPerformed(ActionEvent e) {
        sellerService();
      }
    });

    buyerButton.addActionListener(new ActionListener() {
      @Override
      public void actionPerformed(ActionEvent e) {
        buyerService();
      }
    });

    panel.add(sellerButton);
    panel.add(buyerButton);

    frame.add(panel);
    frame.pack();
    frame.setLocationRelativeTo(null);
    frame.setVisible(true);
```  
**buyerService() 메서드는 모든 구매자 관련 작업을 처리합니다.  **
```
 public void buyerService() {
	    do {
	        String[] options = {"구매하기", "뒤로가기"};
	        int sel3 = JOptionPane.showOptionDialog(
	                null,
	                "구매 시스템입니다.\n기능을 선택하세요.",
	                "구매자",
	                JOptionPane.DEFAULT_OPTION,
	                JOptionPane.PLAIN_MESSAGE,
	                null,
	                options,
	                options[0]
	        );

	        if (sel3 == 0) {
	            // Retrieve available product information
	            String query = "SELECT info FROM products";
	            try {
	                pstmt = conn.prepareStatement(query);
	                rs = pstmt.executeQuery();

	                StringBuilder availableProducts = new StringBuilder("구매할 상품정보를 선택하세요:\n");
	                while (rs.next()) {
	                    String info = rs.getString("info");
	                    availableProducts.append("- ").append(info).append("\n");
	                }

	                String title = JOptionPane.showInputDialog(availableProducts.toString());
	                String getProductQuery = "SELECT * FROM products WHERE info = ?";

	                try {
	                    pstmt = conn.prepareStatement(getProductQuery);
	                    pstmt.setString(1, title);
	                    rs = pstmt.executeQuery();

	                    if (rs.next()) {
	                        String no = rs.getString("no");
	                        String info = rs.getString("info");
	                        int price = rs.getInt("price");
	                        int amount = rs.getInt("amount");
	                        int tPrice = rs.getInt("tPrice");

	                        int num = Integer.parseInt(JOptionPane.showInputDialog("구매할 갯수를 입력하세요:"));
	                        if (num <= amount) {
	                            int totalPrice = tPrice * num;

	                            // Update the stock quantity
	                            String updateQuery = "UPDATE products SET amount = amount - ? WHERE no = ?";
	                            PreparedStatement updateStatement = conn.prepareStatement(updateQuery);
	                            updateStatement.setInt(1, num);
	                            updateStatement.setString(2, no);
	                            updateStatement.executeUpdate();

	                            JOptionPane.showMessageDialog(
	                                    null,
	                                    "구매가 완료되었습니다.\n구매 결과:\n상품명: " + info + "\n구매 수량: " + num + "\n총 가격: " + totalPrice
	                            );
	                        } else {
	                            JOptionPane.showMessageDialog(null, "상품의 재고가 부족합니다.");
	                        }
	                    } else {
	                        JOptionPane.showMessageDialog(null, "해당 상품이 존재하지 않습니다.");
	                    }

	                } catch (SQLException e) {
	                    e.printStackTrace();
	                }

	            } catch (SQLException e) {
	                e.printStackTrace();
	            }

	        } else if (sel3 == 1) {
	            // 뒤로가기
	            break;
	        }
	    } while (true);
	}
```
