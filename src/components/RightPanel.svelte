<script>
    import { gridState } from '$lib/stores/gridState';
    
    // パターンの最大値
    const MAX_BUTTON_VALUE = 12;

    // gridStateの値と表示文字列のマップ
    const displayMap = {
        0: '—',
        1: 'チェア↑',
        2: 'チェア→',
        3: 'チェア↓',
        4: 'チェア←',
        5: 'テーブル↑',
        6: 'テーブル→',
        7: 'テーブル↓',
        8: 'テーブル←',
        9: 'クローゼット↑',
        10: 'クローゼット→',
        11: 'クローゼット↓',
        12: 'クローゼット←',
    };

    // gridStateの値と色のマップ
    const colorMap = {
        0: '#f9f9f9',
        1: '#aaffaa',
        2: '#77dd77',
        3: '#44bb44',
        4: '#119911',
        5: '#aaaaff',
        6: '#7777dd',
        7: '#4444bb',
        8: '#111199',
        9: '#ffaaaa',
        10: '#dd7777',
        11: '#bb4444',
        12: '#991111',
    };

    // ボタンクリック時の処理
    function handleClick(index) {
        gridState.update(currentGrid => {
            const nextValue = currentGrid[index] < MAX_BUTTON_VALUE
                ? currentGrid[index] + 1
                : 0;
            const newGrid = [...currentGrid];
            newGrid[index] = nextValue;
            return newGrid;
        });
    }

    // `gridState`の値を直接参照するためのヘルパー関数
    function getButtonValue(index) {
        return $gridState[index] !== undefined ? $gridState[index] : 0;
    }

    // ボタンに表示する文字列を取得する関数
    function getDisplayValue(index) {
        const value = getButtonValue(index);
        return displayMap[value];
    }

    // ボタンの背景色を取得する関数
    function getBackgroundColor(index) {
        const value = getButtonValue(index);
        return colorMap[value];
    }
</script>

<div>
    <table>
        <tbody>
            {#each Array(8) as _, row}
                <tr>
                    {#each Array(8) as _, col}
                        <td>
                            <button
                                on:click={() => handleClick(row * 8 + col)}
                                aria-label="Grid button at row {row} col {col}"
                                style="background-color: {getBackgroundColor(row * 8 + col)};"
                            >
                                {getDisplayValue(row * 8 + col)}
                            </button>
                        </td>
                    {/each}
                </tr>
            {/each}
        </tbody>
    </table>
</div>

<style>
    button {
        /* 円形にするため、widthとheightを同じ値に設定 */
        width: 80px; 
        height: 80px; /* 縦長にならないように幅に合わせる */
        
        font-size: 12px;
        border: 1px solid #ccc;
        cursor: pointer;
        
        /* 円形にする */
        border-radius: 50%;
        
        /* テキストが中央に配置され、オーバーフローしても丸いボタン内に収まるように調整 */
        white-space: normal; /* 文字列を折り返す */
        word-break: break-word; /* 長い単語を途中で改行 */
        text-align: center;
        display: flex; /* flexboxで中央寄せ */
        align-items: center;
        justify-content: center;
        
        padding: 5px; /* 円形の内部のパディングを調整 */
        box-sizing: border-box;
    }
    
    button:hover {
        filter: brightness(0.9);
    }
    
    table {
        border-collapse: collapse;
    }
    
    td {
        padding: 2px; /* セル間に少しスペースを開ける */
        border: none;
    }
</style>