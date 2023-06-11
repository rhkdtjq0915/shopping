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
buyerService() 메서드는 모든 구매자 관련 작업을 처리합니다.  
main()ShoppingUI 메서드는 새 개체를 만들고 주 메뉴를 표시합니다.**

**먼저 참고한 프로그램 소스 입니다.**
```

```
# 수정사항
**기본적인 틀만 가져왔기 때문에 GUI를 활용하여 프로그램의 가독성을 높이고 기존 프로그램에서는 프로그램 종료 시 데이터가 보존되지 않는 불편함이 있기 때문에 데이터베이스를 활용하여 데이터가 보존될 수 있도록 수정하였습니다.**  

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
