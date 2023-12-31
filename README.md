# 쇼핑 물품 관리 프로그램 만들기

### JAVA언어와 JDBC 활용
### GUI와 이벤트 활용
***

# 주요기능

## 판매자는 다음을 수행할 수 있습니다.
* 새 제품을 등록합니다.
* 기존 제품을 삭제합니다.
* 모든 제품을 나열합니다.
* 상품 번호로 제품을 검색합니다.   


## 구매자는 다음을 수행 할 수 있습니다.
* 제품 확인 후 구입할 수 있습니다.
-------

# 코드 
   
**참고한 프로그램 소스 입니다.**


(https://yongku.tistory.com/entry/%EC%9E%90%EB%B0%94Java-%EC%9E%AC%EA%B3%A0-%EA%B4%80%EB%A6%AC-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%A8-%EC%86%8C%EC%8A%A4)
# 수정사항

**기본적인 틀만 가져왔기 때문에 GUI를 활용하여 프로그램의 편의성을 높이고 기존 프로그램에서는 프로그램 종료 시 데이터가 보존되지 않는 불편함이 있어 데이터베이스를 활용하여 데이터가 보존될 수 있도록 수정해 보았습니다.**  
  
**이 프로그램은 MySQL 데이터베이스를 사용하여 제품 정보를 저장합니다. 데이터베이스 연결은 생성자에서 설정됩니다.**  
  
**main()ShoppingUI 메서드는 새 개체를 만들고 주 메뉴를 표시합니다.**  


```
// 데이터베이스 연결
	  try {
          conn = DriverManager.getConnection(DB_URL, DB_USERNAME, DB_PASSWORD);
      } catch (SQLException e) {
          e.printStackTrace();
      }
```
**메서드를 호출하여 데이터베이스에 연결하려고 합니다. 변수는 데이터베이스의 URL을 포함하는 문자열입니다. 변수는 데이터베이스의 사용자 이름을 포함하는 문자열입니다. 변수는 데이터베이스의 암호를 포함하는 문자열입니다. 연결에 성공하면 변수에 객체가 할당됩니다. 연결에 실패하면 예외가 throw됩니다.**

```
    frame = new JFrame("Shopping");
```
**개체를 만들고 제목을 "Shopping"으로 설정합니다. 개체는 GUI(그래픽 사용자 인터페이스) 창입니다.**

```
    frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
```
**개체에 대한 기본 닫기 작업을 로 설정합니다. 즉, 사용자가 개체를 닫으면 응용 프로그램이 종료됩니다.**
```
    panel = new JPanel();
``` 
**개체를 만듭니다. 객체는 GUI 구성 요소의 컨테이너입니다.**
```
    
    sellButton = new JButton("판매자");
    buyButton = new JButton("구매자");
```
**두 개의 개체를 만듭니다. 객체는 텍스트를 표시하고 사용자가 클릭할 때 작업을 수행하는 데 사용할 수 있는 GUI 구성 요소입니다. 객체에는 "판매자"(판매자)라는 레이블이 지정되고 객체에는 "구매자"(구매자)라는 레이블이 지정됩니다.**
```
    sellButton.addActionListener(new ActionListener() {
      @Override
      public void actionPerformed(ActionEvent e) {
        sellService();
      }
    });

    buyButton.addActionListener(new ActionListener() {
      @Override
      public void actionPerformed(ActionEvent e) {
        buyService();
      }
    });
```
**버튼을 눌렀을때 해당하는 메소드를 호출하는 함수 입니다.이 메서드는 사용자가 개체를 클릭할 때 호출됩니다.**
  
-----
## sellService() 는 판매자에게 다음 기능을 제공합니다.
* 제품 등록: 판매자는 제품 번호, 정보, 가격, 금액 및 배송비를 제공하여 새 제품을 등록할 수 있습니다.
* 제품 삭제: 판매자는 제품 번호를 제공하여 기존 제품을 삭제할 수 있습니다.
* 제품 목록: 판매자는 등록된 모든 제품 목록을 볼 수 있습니다.
* 제품 검색: 판매자는 제품 번호를 제공하여 제품을 검색할 수 있습니다.
* 뒤로 가기: 판매자는 이전 화면으로 돌아갈 수 있습니다. 

## 각 기능에 대한 자세한 설명
* 상품 등록: 판매자는 다음 정보를 입력하여 새 제품을 등록할 수 있습니다.
* 판매자가 정보를 입력하면 데이터베이스의 테이블에 새 행을 삽입합니다.
	- 상품 번호: 상품의 고유 식별자입니다.
	- 상품 정보: 상품에 대한 간략한 설명입니다.
	- 가격: 상품의 가격입니다.
	- 금액: 판매 가능한 상품의 갯수입니다.
	- 배송비: 고객에게 상품을 배송하는 비용입니다.
* 제품 삭제: 판매자는 제품 번호를 입력하여 기존 제품을 삭제할 수 있습니다. 그런 다음 함수는 테이블에서 행을 삭제합니다.
* 제품 목록: 판매자는 함수를 호출하여 현재 등록된 모든 제품 목록을 볼 수 있습니다. 이 함수는 각 객체가 단일 제품을 나타내는 객체의 a를 반환합니다.
* 제품 검색: 판매자는 제품 번호를 입력하여 특정 제품을 검색할 수 있습니다. 그런 다음 함수는 제품이 발견되거나 제품을 찾을 수 없는 경우 개체를 반환합니다.
```
  public void sellService() {
    // 판매자 관련 로직 구현

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
-----
## buyService() 메소드는 구매자에게 다음 기능을 제공합니다.
* 제품 구매: 구매자는 사용 가능한 제품 목록에서 제품을 선택하고 구매하려는 수량을 입력하여 제품을 구매할 수 있습니다.
* 뒤로 가기: 구매자는 이전 화면으로 돌아갈 수 있습니다.   

## 
* 먼저 사용자에게 사용 가능한 제품 목록을 표시합니다.
* 사용자가 목록에서 제품을 선택합니다.
* 제품 이름, 가격, 수량 및 총 가격을 포함하여 데이터베이스에서 제품 정보를 검색합니다.
* 사용자에게 구매하려는 수량을 입력하라는 메시지를 표시합니다.
* 요청된 수량을 사용할 수 있는지 확인합니다. 이 경우 함수는 데이터베이스의 재고 수량을 업데이트하고 구매를 확인하는 메시지를 표시합니다. 
* 요청된 수량을 사용할 수 없는 경우 함수는 제품의 재고가 없음을 나타내는 메시지를 표시합니다.
  
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
-----

