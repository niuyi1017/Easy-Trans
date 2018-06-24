<template>
    <div>
        <Layout>
            <Header id="header">
                <div class="headerBar"><h1>{{header}}</h1></div>
            </Header>
       
            <Content id = 'content'>
                
                <Tabs id="tab" value="name1">
                    <TabPane label="翻译" name="name1">
                        <tab1></tab1>
                    </TabPane>
                    <TabPane label="单词本" name="name2">标签二的内容</TabPane>
                    <TabPane label="我的" name="name3">标签三的内容</TabPane>
                    
                </Tabs>
                

            </Content>

            <Footer>
                
            </Footer>
       
        </Layout>
        
        
        
    </div>
</template>
<script>
import tab1 from './tab1'
export default {
    name:'translation',
    components: {
    tab1
    },
    data(){
        return{
            header:'Easy Trans'
        }
    }
    
}
</script>
<style scoped>


.headerBar{
   margin: 20px;
    color: #f4615c
}
@media screen and (min-width: 414px) {
   #content{
    width: 420px;
    position: absolute;
    left: 50%;
    margin-left: -210px;
    top:60px;
}
}


#tab{
    margin: 20px 10px;
   
}


</style>

