<template>
  <!-- html -->
  <div id="app">
    <h1>Vueで五目並べ</h1>

    <table id="table">
      <!-- 19*19マスの描写 -->
      <tr v-for="(row, rowsIndex) in states" :key="row">
        <td
          v-for="(state, colsIndex) in row"
          :key="state"
          @click="onSelect(rowsIndex, colsIndex)"
        >
          <div style="color: #00000" v-if="state == 1">○</div>
          <div style="color: #00000" v-if="state == 2">●</div>
        </td>
      </tr>
    </table>
    <br /><!-- 空白 -->
    <div style="text-align: center">
      <!-- 次の人の指示 -->
      <div style="color: #00000" v-if="playerId == 1">
        「○ プレイヤーさんの番です」
      </div>
      <div style="color: #00000" v-if="playerId == 2">
        「● プレイヤーさんの番です」
      </div>
    </div>
    <br />
    <div v-if="winnerText" style="text-align: center">
      <h2 v-text="winnerText"></h2>
      <!-- <img src="./images/congrats.png">  -->
      <input
        type="button"
        value="もう一度プレイする"
        onclick="window.location.reload();"
      />
    </div>
    <input type="button" value="リセット" onclick="window.location.reload();" />
  </div>
</template>

<script>
//javascript

export default {
  name: "Gomoku",
  data() {
    return {
      states: [], // 初期値はmountedの中で設定（マス目の状態）
      playerId: 1, // ○ のプレイヤーから開始（選択できるプレイヤーのID）
      winnerText: "",
    };
  },
  methods: {
    init: function () {
        this.states = [
        [-1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,],
        [-1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,],
        [-1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,],
        [-1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,],
        [-1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,],
        [-1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,],
        [-1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,],
        [-1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,],
        [-1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,],
        [-1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,],
        [-1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,],
        [-1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,],
        [-1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,],
        [-1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,],
        [-1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,],
        [-1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,],
        [-1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,],
        [-1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,],
        [-1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,],
      
       //初期状態
      ];
    },
    /* マスを押したら */
    onSelect: function (rowsIndex, colsIndex) {
      this.winnerText = "";

      if (this.states[rowsIndex][colsIndex] != -1) {
        alert("そのマスは、すでに選択されています！"); //選択済
      } else {
        var states = JSON.parse(JSON.stringify(this.states));
        states[rowsIndex][colsIndex] = this.playerId;
        this.states = states;
        this.playerId = this.playerId == 1 ? 2 : 1;
        var winnerId = this.getWinnerId(); /* getWinnerIDで勝敗を決めてwinnerIDへ代入 */
        console.log(winnerId);
        if (winnerId != -1) {
          
          const playerIds = {
            1: "○",
            2: "●",
          };
          this.winnerText =
            playerIds[winnerId] + " さんの勝ちです。おめでとうございます！";//勝利
        } else if (this.isDraw()) {
          this.init();
          alert('引き分けです！');//引き分け
        }
      }
    },
    /*勝敗を決める関数*/
    getWinnerId: function () {
      /* for (var i = 0; i < 19; i++) {
        // 横の列
        var row = this.states[i];
        if (this.isStatesFilled(row)) {
          return row[0];
        }

        // 縦の列
        var col = [
          this.states[0][i],
          this.states[1][i],
          this.states[2][i],
          this.states[3][i],
          this.states[4][i],
          this.states[5][i],
          this.states[6][i],
          this.states[7][i],
          this.states[8][i],
          this.states[9][i],
          this.states[10][i],
          this.states[11][i],
          this.states[12][i],
          this.states[13][i],
          this.states[14][i],
          this.states[15][i],
          this.states[16][i],
          this.states[17][i],
          this.states[18][i],
        ];
        if (this.isStatesFilled(col)) {
          return this.states[0][i];
        }
      }

      // ななめ
      var skew1 = [
        this.states[0][i],
        this.states[1][i],
        this.states[2][i],
        this.states[3][i],
        this.states[4][i],
        this.states[5][i],
        this.states[6][i],
        this.states[7][i],
        this.states[8][i],
        this.states[9][i],
        this.states[10][i],
        this.states[11][i],
        this.states[12][i],
        this.states[13][i],
        this.states[14][i],
        this.states[15][i],
        this.states[16][i],
        this.states[17][i],
        this.states[18][i],
      ];
      if (this.isStatesFilled(skew1)) {
        return this.states[0][0];
      }

      var skew2 = [this.states[0][2], this.states[1][1], this.states[2][0]];
      if (this.isStatesFilled(skew2)) {
        return this.states[0][2];
      }

      return -1; */
      for(var x=0; x<19; x++){
            for(var y=0; y<19; y++){
                if(this.states[x][y] === 1 ){
                    /*縦下5つが同じ*/
                    if(this.states[x][y] == this.states[x][y+1]&& this.states[x][y+1] == this.states[x][y+2]&& this.states[x][y+2] == this.states[x][y+3]&& this.states[x][y+3] == this.states[x][y+4])//下縦5つが同じ
                    {
                        
                        return 1;
                        
                    }
                    /*横右5つが同じ*/
                    else if(this.states[x][y] == this.states[x+1][y]&& this.states[x+1][y] == this.states[x+2][y]&& this.states[x+2][y] == this.states[x+3][y]&& this.states[x+3][y] == this.states[x+4][y])
                    {
                        return 1;
                    }
                    else if(this.states[x][y] == this.states[x+1][y+1]&& this.states[x+1][y+1] == this.states[x+2][y+2]&& this.states[x+2][y+2] == this.states[x+3][y+3]&& this.states[x+3][y+3] == this.states[x+4][y+4])
                    {
                        return 1;
                    }
                    else if(this.states[x][y] == this.states[x-1][y+1]&& this.states[x-1][y+1] == this.states[x-2][y+2]&& this.states[x-2][y+2] == this.states[x-3][y+3]&& this.states[x-3][y+3] == this.states[x-4][y+4])
                    {
                        return 1;
                    }
                    
                    
                    
                    
                }
                else if(this.states[x][y] === 2){
                    
                    if(this.states[x][y] == this.states[x][y+1]&& this.states[x][y+1] == this.states[x][y+2]&& this.states[x][y+2] == this.states[x][y+3]&& this.states[x][y+3] == this.states[x][y+4])//下縦5つが同じ
                    {
                        return 2;
                    }
                    /*右5つが同じ*/
                    else if(this.states[x][y] == this.states[x+1][y]&& this.states[x+1][y] == this.states[x+2][y]&& this.states[x+2][y] == this.states[x+3][y]&& this.states[x+3][y] == this.states[x+4][y])
                    {
                        return 2;
                    }
                    else if(this.states[x][y] == this.states[x+1][y+1]&& this.states[x+1][y+1] == this.states[x+2][y+2]&& this.states[x+2][y+2] == this.states[x+3][y+3]&& this.states[x+3][y+3] == this.states[x+4][y+4])
                    {
                        return 2;
                    }
                     else if(this.states[x][y] == this.states[x-1][y+1]&& this.states[x-1][y+1] == this.states[x-2][y+2]&& this.states[x-2][y+2] == this.states[x-3][y+3]&& this.states[x-3][y+3] == this.states[x-4][y+4])
                    {
                        return 2;
                    }
                    
                }
                
            }
        }
        return -1;
    },
    isStatesFilled: function (states) {
      return (
        //-1が残っていても勝敗はつく
        states[0] != -1 && states[0] == states[1] && states[1] == states[2]
      );
    },
    /*選ばれていないマス(-1)が無くなって勝利条件を満たさなければ引き分け判定(引き分けならばtrue)*/
    isDraw: function () {
      for (var i in this.states) {
        var row = this.states[i];

        for (var j in row) {
          var state = row[j];

          if (state == -1) {
            return false; //まだ-1が残っているので続ける余地がある
          }
        }
      }

      return true; //引き分け判定
    },
  },
  mounted: function () {
    this.init();
  },
};
</script>

<style scoped>
/*css*/
#table {
  margin: 0 auto;
  border-collapse: collapse;
  border: 3px solid #000000;
}

#table td {
  border: 1px solid #000000;
  height: 30px;
  width: 30px;
  text-align: center;
  vertical-align: middle;
  font-size: 22px;
  cursor: pointer;
}
</style>