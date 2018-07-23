<template>
    <div>
        <RadioGroup v-model="from" type="button" size="small">
            <Radio label="auto">自动识别</Radio>
            <Radio label="ja">日汉</Radio>
            <Radio label="ko">韩汉</Radio>
            <Radio label="fr">法汉</Radio>
            <Radio label="ru">俄汉</Radio>
            <Radio label="pt">葡汉</Radio>
            <Radio label="es">西汉</Radio>
            
        </RadioGroup>
        
        <Input class="query"  v-model="query" placeholder="请输入单词或句子..." clearable style="width: 90%"></Input>

        <transition name="fade">
            <div class="result" v-if="query!=''">
                <div class="res-head" >
                    <strong v-if="!result.longtext">{{query}} </strong> 
                    <strong v-else>长文本</strong> 
                    
                    <span v-if="result.basic.phonetic">({{result.basic.phonetic}})</span>
                    <Icon id="add" type="plus-round"></Icon>
                </div>
                <div class="res-desc" v-if="!result.longtext">
                     
                   
                    <div class="card">
                        <p>基本释义：</p>
                        
                        <ul>
                            <li v-for="(item, index) in result.basic.explains" :key="index">{{item}}</li>
                        </ul>
                    </div> 
                     <div class="card" >
                        <p>网络示例：</p>
                        
                        <ul>
                            
                            <li v-for="(value,index) in result.web" :key="index">{{value.key}}：{{value.value}}</li>
                           
                        </ul>
                    </div> 
                    <div class="card" >
                        <p>翻译：</p>
                        
                        <ul>
                            
                            <li>{{result.trans[0]}}</li>
                           
                        </ul>
                    </div>
                    <!-- <div class="card" >
                        <p>发音：</p>
                        
                            <div class="audiowaper">
                                <Icon type="play"></Icon>
                                <audio :src="result.tSpeakUrl" autoplay="autoplay"> </audio>
                               
                            </div>
                            
                           
                       
                    </div>           -->
                        
                </div>

                <div v-if="result.longtext" class="longtext">
                    <div class="card" >
                        <p>原文：</p>
                        
                        <div class="text"> {{query}}</div>
                            
                           
                           
                        
                    </div> 
                    <div class="card" >
                        <p>翻译：</p>
                        <ul>
                           
                             <div class="text">  {{result.trans[0]}}</div>
                           
                        </ul>
                    </div> 
                    <!-- <div class="card" >
                        <p>发音：</p>
                        
                        <ul>
                            
                            <li><audio :src="result.tSpeakUrl" controls="controls">
                                </audio></li>
                           
                        </ul>
                    </div>  -->
                </div>
            </div>
            <div class="background">
                <h3>Easy Trans</h3>
                <p>made with <Icon id="heart" type="ios-heart"></Icon>  by Niu Yi</p>
            </div>
        </transition>
       
        
    </div>
    

    
</template>

<script>
import md5 from 'md5';
import $ from 'jquery';

const appKey = '12a5f4de52040abd';
const key = 'RhqFrLJ3ZUHwEMiyXILlMohWa7S9lzXn';
var salt = (new Date).getTime();
const to = "auto";
var url= 'https://openapi.youdao.com/api';



export default {
    name:'tab1',
    data(){
        return{
            query:'',
            
            from:'auto',
            result:{
              
                basic:{
                    
                },
                web:{},
                webdict:{},
                trans:'',
                longtext:false,
                tSpeakUrl:'',
                
                
            }
        }
    },
    watch:{
        query:function(){
            var self = this
            var str = md5(appKey+self.query+salt+key);
            
            $.ajax({
                url: url,
                type: 'get',
                dataType: 'jsonp',
                jsonpCallback: "showData",
                data: {
                    
                    q: self.query,
                    appKey: appKey,
                    salt: salt,
                    from: self.from,
                    to: to,
                    sign: str
                },
                success: function (data) {
                    if(data.basic){
                        self.result.basic = data.basic;
                    }
                    if(data.web){
                        self.result.web = data.web;
                    }
                   

                    if(data.tSpeakUrl){
                        self.result.tSpeakUrl = data.tSpeakUrl;
                    }

                    if(self.query.split(' ').length>5){
                        self.result.longtext = true;
                        
                    }else if(/^[\u3220-\uFA29]+$/.test(self.query)&&self.query.length>8){
                        self.result.longtext = true;
                        self.result.basic.phonetic = '';
                    }else{
                        self.result.longtext = false;

                    }
                    
                    
                    
                    self.result.trans = data.translation;
                   
                   
                 
                } 
            });
        }
    },
    methods:{
        
    }
}


</script>


<style scoped>
.query{
    margin: 10px;
}
.result{
    margin: 20px;
    border: 1px solid #f4615c;;
    /* height: 400px; */
    border-radius:5px;
    box-shadow:  2px 2px 10px #a2a9b6;
    padding-bottom: 20px;
}
.res-head{
    margin: 5px;
    height: 50px;
    line-height: 50px;
    font-size: 20px;
    color: #4c5161;
    background: #f7f9fa;
    box-shadow:  0px 2px 5px #a2a9b6;
    position: relative;
}
.res-head span{
    font-size: 12px
}
#add{
    position: absolute;
    right: 10%;
    top:30%;
    color: #f4615c;
    
}
.card{
    border:1px solid #f7f9fa; 
    margin: 10px;
    padding: 20px;
    box-shadow:  0px 2px 5px #a2a9b6;
}
.card p{
    border-bottom: 1px solid #f4615c;
    margin-bottom: 5px;
    height: 30px;
    line-height: 30px;
    font-size: 14px;
    font-weight: bold;
    

}
ul{
    list-style: none;
}
ul li {
    height: 25px;
   overflow: hidden;
    line-height: 25px;
    border-bottom: 1px dotted #a2a9b6
}
.text{
    line-height: 25px;
    font-size: 14px;
    padding: 10px

}
.background{
  
    margin-top: 85%;
    padding: 40px;
    color: #a2a9b6;
   
}
#heart{
    color: #f4615c
}


</style>
