# 🏆signatecup2024summer🏄
## ✈️目的✈️
旅行会社の保有する顧客データ（属性や志向、営業担当との接触履歴等）を元に、旅行パッケージの成約率を予測するモデルを構築

## 👥データの説明👥
| ヘッダ名称             | 値例                   | データ型 | 説明                                                             | 
| ---------------------- | ---------------------- | -------- | ---------------------------------------------------------------- | 
| id                     | 0                      | int64    | 顧客ID                                                           | 
| Age                    | 50歳                   | str      | 顧客の年齢                                                       | 
| TypeofContact          | Self Enquiry           | str      | 顧客への連絡・接触方法                                           | 
| CityTier               | 2                      | int64    | 都市層(1>2>3)                                                    | 
| DurationOfPitch        | 900秒                  | str      | 営業担当者による顧客への商品のセールス時間                       | 
| Occupation             | Large Business         | str      | 顧客の職業                                                       | 
| Gender                 | male                   | str      | 顧客の性別                                                       | 
| NumberOfPersonVisiting | 1                      | int64    | 予定している旅行の同行者の数                                     | 
| NumberOfFollowups      | 4                      | float64  | セールス後に営業担当者が行ったフォローアップの回数               | 
| ProductPitched         | Basic                  | str      | 営業担当者のセールスした商品の種類                               | 
| PreferredPropertyStar  | 3                      | float64  | 顧客の希望するホテルのランク                                     | 
| NumberOfTrips          | 5                      | str      | 顧客の年間旅行数                                                 | 
| Passport               | 1                      | int64    | パスポートの所持(0: 不所持、1: 所持)                             | 
| PitchSatisfactionScore | 4                      | int64    | 営業担当者のセールストークに対する顧客の満足度                   | 
| Designation            | Executive              | str      | 顧客の役職                                                       | 
| MonthlyIncome          | 253905                 | str      | 顧客の月収                                                       | 
| customer_info          | 未婚 車未所持 子供なし | str      | 顧客の情報のメモ(婚姻状況や車の有無、旅行への子どもの同伴の有無) | 
| ProdTaken(目的変数)    | 1                      | int64    | 商品の契約状態(0:不成約、1:成約)                                 | 



    """
 
    """
   
Html
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <title>顧客クエリ テスト画面</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 30px; background-color: #f9f9f9; }
        .container { max-width: 600px; background: white; padding: 20px; border-radius: 5px; box-shadow: 0 2px 5px rgba(0,0,0,0.1); }
        .form-group { margin-bottom: 15px; }
        label { display: block; margin-bottom: 5px; font-weight: bold; }
        input { padding: 8px; width: 95%; border: 1px solid #ccc; border-radius: 4px; }
        button { padding: 10px 20px; background-color: #007bff; color: white; border: none; border-radius: 4px; cursor: pointer; }
        button:hover { background-color: #0056b3; }
        pre { background: #333; color: #fff; padding: 15px; border-radius: 4px; overflow-x: auto; font-size: 14px; }
        .result-section { margin-top: 25px; }
    </style>
</head>
<body>
    <div class="container">
        <h2>顧客クエリ テスト画面</h2>
        <p>店番と顧客番号を入力して「JSON作成・保存」を押してください。</p>

<script>
        document.getElementById('queryForm').addEventListener('submit', async function(e) {
            e.preventDefault();
            
            const shopId = document.getElementById('shopId').value;
            const customerId = document.getElementById('customerId').value;
            const resultElement = document.getElementById('result');
            
            resultElement.textContent = "送信中...";

            try {
                const response = await fetch('/api/external-proxy/', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                        'X-CSRFToken': getCookie('csrftoken')
                    },
                    body: JSON.stringify({
                        shop_id: shopId,
                        customer_id: customerId
                    })
                });

                // レスポンスのコンテンツタイプを確認します
                const contentType = response.headers.get("content-type");
                
                if (contentType && contentType.includes("application/json")) {
                    // JSONの場合は綺麗に整形して表示
                    const data = await response.json();
                    resultElement.textContent = JSON.stringify(data, null, 4);
                } else {
                    // JSON以外（HTMLエラー等）が返ってきた場合はそのままテキストとして表示
                    const rawText = await response.text();
                    resultElement.textContent = `【エラーが発生しました】\nステータスコード: ${response.status} ${response.statusText}\n\n${rawText}`;
                }

            } catch (error) {
                resultElement.textContent = "通信自体に失敗しました: " + error;
            }
        });

        // CSRFトークンを取得する関数
        function getCookie(name) {
            let cookieValue = null;
            if (document.cookie && document.cookie !== '') {
                const cookies = document.cookie.split(';');
                for (let i = 0; i < cookies.length; i++) {
                    const cookie = cookies[i].trim();
                    if (cookie.substring(0, name.length + 1) === (name + '=')) {
                        cookieValue = decodeURIComponent(cookie.substring(name.length + 1));
                        break;
                    }
                }
            }
            return cookieValue;
        }
    </script>
"""