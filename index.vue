<template>
  <div>
    <div id="example-1">
      <input v-model="message" placeholder="edit me">
      <button v-on:click="showSeeksGraph2(message)">搜索</button>
    </div>

    <div style="height:calc(100vh - 50px);">
      <RelationGraph ref="seeksRelationGraph" :options="graphOptions" :on-node-click="onNodeClick" :on-line-click="onLineClick" />
    </div>
  </div>
</template>

<script>
const nodes1=null;
const links1=null;
import RelationGraph from 'relation-graph';
import $ from 'jquery';
export default {
  nodes1,
  links1,
  name: 'Relationship',
  components: { RelationGraph },
  data() {
    return {
      data11:[],
      data22:[],
      nodes:[],
      links:[],
      nodes1:[
        // node配置选项：http://relation-graph.com/#/docs/node
        // node支持通过插槽slot完全自定义，示例：http://relation-graph.com/#/demo/adv-slot
        { id: 'a', text: 'A', borderColor: 'yellow' },
        { id: 'b', text: 'B', color: '#43a2f1', fontColor: 'yellow' },
        { id: 'c', text: 'C', nodeShape: 1, width: 80, height: 60 },
        { id: 'e', text: 'E', nodeShape: 0, width: 150, height: 150 }
      ],
      links2:[
        // link配置选项：http://relation-graph.com/#/docs/link
        { from: 'a', to: 'b', text: '关系1', color: '#43a2f1' },
        { from: 'a', to: 'c', text: '关系2' },
        { from: 'a', to: 'e', text: '关系3' },
        { from: 'b', to: 'e', color: '#67C23A' }
      ],
     counter: 0,
      graphOptions: {
        allowSwitchLineShape: true,
        allowSwitchJunctionPoint: true,
        defaultJunctionPoint: 'border'
        // 这里可以参考"Graph 图谱"中的参数进行设置:http://relation-graph.com/#/docs/graph
      }
    }
  },
  created() {
    this.getRelationshipData();
  },
  mounted() {
    this.showSeeksGraph();
  },
  methods: {
    getRelationshipData(){

    },
    showSeeksGraph() {
      var data1 = [];
      var data2 = [];
      //执行Ajax请求
      $.ajax({
        xhrFields: { withCredentials: true },
        crossDomain: true,
        type: "POST",
      //  url: "http://localhost:8080/getRelationshipData",
        url: "http://localhost:8080/getResult",
        async: false,
        dataType: "json",
        contentType: "application/json; charset=UTF-8",
        data: JSON.stringify({
          exec1:null,
          exec2: null
        }),
        success: function (result) {
          for (let i in result.nodes1) {
            let o = {
              id:result.nodes1[i].id,
              text:result.nodes1[i].text,
              color:result.nodes1[i].color
            };


            //o[i] = data[i];
            data1.push(o)
          }
               // 某种意义上来说，数组也是object
          for (let i in result.links2) {
            let o = {
              from:result.links2[i].from,
              text:result.links2[i].text,
              to:result.links2[i].to
            };
            //o[i] = data[i];
            data2.push(o)
          }
          this.nodes = data1;
          this.links =  data2;
        }
      });
      console.log("====showSeeksGraph初始化:"+this.nodes);
      console.log("====data1:"+data1);
      console.log("====data2:"+data2);
      var __graph_json_data = {
        rootId: '200',
        nodes: data1,
        links: data2,
      }
      this.$refs.seeksRelationGraph.setJsonData(__graph_json_data, (seeksRGGraph) => {
        // Called when the relation-graph is completed
      })

    },
    showSeeksGraph2(msg) {

          var data1 = [];
          var data2 = [];
          //执行Ajax请求
          $.ajax({
            xhrFields: { withCredentials: true },
            crossDomain: true,
            type: "POST",
          //  url: "http://localhost:8080/getRelationshipData",
            url: "http://localhost:8080/getResult",
            async: false,
            dataType: "json",
              contentType: "application/json; charset=UTF-8",
                data: JSON.stringify({
                exec1:msg,
                exec2: null
              }),

                success: function (result) {
                if(result.nodes1.length==0){
                  setTimeout(()=>{
                    alert("节点不存在")
                    location.reload()
                  },5000)
                }
              for (let i in result.nodes1) {
                let o = {
                  id:result.nodes1[i].id,
                  text:result.nodes1[i].text,
                };
                //o[i] = data[i];
                data1.push(o)
              }
                   // 某种意义上来说，数组也是object
              // for (let i in result.relTexlist) {
              //   let o = {
              //     from:result.links2[i].from,
              //     text:result.links2[i].text,
              //     to:result.links2[i].to
              //   };
                //o[i] = data[i];
              //   data2.push(o)
              // }
              this.nodes = data1;
              // this.links = data2;

            },


          });
          var __graph_json_data = {
            rootId: '0',
            nodes: data1,
            links: data2
          }
          this.$refs.seeksRelationGraph.setJsonData(__graph_json_data, (seeksRGGraph) => {
            // Called when the relation-graph is completed
          })

        },
    onNodeClick(nodeObject, $event) {
      console.log('onNodeClick:', nodeObject);
      this.openNewWindwos(nodeObject);
    },
    onLineClick(lineObject, $event) {
      console.log('onLineClick:', lineObject)
    },
    openNewWindwos(nodeObject){
      window.open('../#/characterView'+'?id='+nodeObject.text,'_blank');
    }
  }
}
</script>
