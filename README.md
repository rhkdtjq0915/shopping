쇼핑 물품 관리 프로그램 만들기
-------------
### Java 언어와 JDBS 활용
### GUI와 이벤트를 활용
### jdbs 연결
***
판매자는 다음을 수행할 수 있습니다.
-------------
* 새 제품을 등록합니다.
* 기존 제품을 삭제합니다.
* 모든 제품을 나열합니다.
* ID로 제품을 검색합니다.

## class
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
