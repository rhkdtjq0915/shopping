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

   
**먼저 참고한 프로그램 소스 입니다.**
```

```
# 수정사항
**기본적인 틀만 가져왔기 때문에 GUI를 활용하여 프로그램의 편의성을 높이고 기존 프로그램에서는 프로그램 종료 시 데이터가 보존되지 않는 불편함이 있어 데이터베이스를 활용하여 데이터가 보존될 수 있도록 수정해 보았습니다.**  
**main()ShoppingUI 메서드는 새 개체를 만들고 주 메뉴를 표시합니다.**  

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
**buyService() 메서드는 모든 구매자 관련 작업을 처리합니다.**
```
 public void buyService() {
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
**sellService() 함수는 판매자에게 다음 기능을 제공하는 Java 함수입니다.**
* 제품 등록: 판매자는 제품 번호, 정보, 가격, 금액 및 배송비를 제공하여 새 제품을 등록할 수 있습니다.
* 제품 삭제: 판매자는 제품 번호를 제공하여 기존 제품을 삭제할 수 있습니다.
* 제품 목록 보기: 판매자는 등록된 모든 제품 목록을 볼 수 있습니다.
* 제품 검색: 판매자는 제품 번호를 제공하여 제품을 검색할 수 있습니다.
* 뒤로 가기: 판매자는 이전 화면으로 돌아갈 수 있습니다. 
```
  public void sellService() {
    // 판매자 관련 로직 구현
    // 예: 판매자 화면을 보여주고 필요한 기능을 구현
	  String[] options = {"등록", "삭제", "목록", "검색", "뒤로가기"};
	    int sel2 = JOptionPane.showOptionDialog(
	        null,
	        "판매자 관리시스템입니다.\n기능을 선택하세요.",
	        "판매자",
	        JOptionPane.DEFAULT_OPTION,
	        JOptionPane.PLAIN_MESSAGE,
	        null,
	        options,
	        options[0]
	    );

	    switch (sel2) {
	      case 0:
	        // 등록
              try {
                  String no = JOptionPane.showInputDialog("상품번호를 입력하세요:");
                  String info = JOptionPane.showInputDialog("상품정보를 입력하세요:");
                  int price = Integer.parseInt(JOptionPane.showInputDialog("상품가격을 입력하세요:"));
                  int amount = Integer.parseInt(JOptionPane.showInputDialog("상품수량을 입력하세요:"));
                  int tPrice = Integer.parseInt(JOptionPane.showInputDialog("배송비용을 입력하세요:"));

                  String query = "INSERT INTO products (no, info, price, amount, tPrice) VALUES (?, ?, ?, ?, ?)";
                  pstmt = conn.prepareStatement(query);
                  pstmt.setString(1, no);
                  pstmt.setString(2, info);
                  pstmt.setInt(3, price);
                  pstmt.setInt(4, amount);
                  pstmt.setInt(5, tPrice);
                  pstmt.executeUpdate();

                  JOptionPane.showMessageDialog(null, "상품이 등록되었습니다.");
              } catch (SQLException e) {
                  e.printStackTrace();
              }
              break;

	      case 1:
	        // 삭제
	    	  try {
                  String deleteNo = JOptionPane.showInputDialog("삭제할 상품번호를 입력하세요:");

                  String query = "DELETE FROM product WHERE no = ?";
                  pstmt = conn.prepareStatement(query);
                  pstmt.setString(1, deleteNo);
                  int deletedRows = pstmt.executeUpdate();

                  if (deletedRows > 0) {
                      JOptionPane.showMessageDialog(null, "상품이 삭제되었습니다.");
                  } else {
                      JOptionPane.showMessageDialog(null, "상품을 찾을 수 없습니다.");
                  }
              } catch (SQLException e) {
                  e.printStackTrace();
              }
              break;

	      case 2:
	        // 목록
              try {
                  String query = "SELECT * FROM products";
                  pstmt = conn.prepareStatement(query);
                  rs = pstmt.executeQuery();

                  StringBuilder productList = new StringBuilder();
                  while (rs.next()) {
                      String no = rs.getString("no");
                      String info = rs.getString("info");
                      int price = rs.getInt("price");
                      int amount = rs.getInt("amount");
                      int tPrice = rs.getInt("tPrice");

                      productList.append("상품번호: ").append(no).append("\n");
                      productList.append("상품정보: ").append(info).append("\n");
                      productList.append("상품가격: ").append(price).append("\n");
                      productList.append("상품수량: ").append(amount).append("\n");
                      productList.append("배송비용: ").append(tPrice).append("\n");
                      productList.append("--------------------\n");
                  }

                  JOptionPane.showMessageDialog(null, productList.toString());
              } catch (SQLException e) {
                  e.printStackTrace();
              }
              break;

	      case 3:
	        // 검색
              try {
                  String searchNo = JOptionPane.showInputDialog("검색할 상품번호를 입력하세요:");
                  String query = "SELECT * FROM products WHERE no = ?";
                  pstmt = conn.prepareStatement(query);
                  pstmt.setString(1, searchNo);
                  rs = pstmt.executeQuery();

                  if (rs.next()) {
                      String no = rs.getString("no");
                      String info = rs.getString("info");
                      int price = rs.getInt("price");
                      int amount = rs.getInt("amount");
                      int tPrice = rs.getInt("tPrice");

                      StringBuilder productInfo = new StringBuilder();
                      productInfo.append("상품번호: ").append(no).append("\n");
                      productInfo.append("상품정보: ").append(info).append("\n");
                      productInfo.append("상품가격: ").append(price).append("\n");
                      productInfo.append("상품수량: ").append(amount).append("\n");
                      productInfo.append("배송비용: ").append(tPrice).append("\n");

                      JOptionPane.showMessageDialog(null, productInfo.toString());
                  } else {
                      JOptionPane.showMessageDialog(null, "상품을 찾을 수 없습니다.");
                  }
              } catch (SQLException e) {
                  e.printStackTrace();
              }
              break;

	      case 4:
	        // 뒤로가기
	        break;
	    }
	  }
```
