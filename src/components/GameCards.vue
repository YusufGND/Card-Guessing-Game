<template>
   <div class="Game-Area"> 
       <p> {{ selectedCard}}</p>
       <h1 class="title">Poğaça <span>Nerede</span><strong>?</strong> </h1> 
       <h4 class="description">Açık Kartlardan Birini seçtikten sonra,kapalı olan karta tıklayınız</h4>
       <div class="container">
 <!--(satır 9) selectedCard => card.id'ye eşit ise 'shadow' isimlik class'ı ekle-->  
        <transition-group name="rotate-all" appear class="card-container">
            <app-card 
           :class="{'shadow' : selectedCard == card.id}"
           @click.native="selectedCard = card.id"
           v-for="card in cards" 
           :card = "card" 
           :key="card.id" 
           ></app-card>     
        </transition-group>   <!-- x'KEY'x-->   
       </div>
       <div class="container">
           <transition name="rotate" mode="out-in"> 
           <component 
           @click.native="showCard(answer)"
           :card="answer"
           :is="activeCard"
           ></component>
           </transition>
       </div>
   </div>
</template>
<script>    
import Card from "./Card.vue"; 
import DefaultCard from "./DefaultCard.vue" ;
export default {
    components:
    {
        appCard: Card,
        appDefaultCard : DefaultCard,
    },
    data()
    {
        return {
            selectedCard : null, //Seçilen Kart
            answer : {},
            activeCard : "app-default-card",
            cards: [
                {id : 1, component: "app-card" , image: "/src/assets/card-1.jpg"}, //Card Images
                {id : 2, component: "app-card" , image: "/src/assets/card-2.jpg"},
                {id : 3, component: "app-card" , image: "/src/assets/card-3.jpg"},
                {id : 4, component: "app-card" , image: "/src/assets/card-4.jpg"},
                {id : 5, component: "app-card" , image: "/src/assets/card-5.jpg"},
            ]
        }
    },
    created()
    {
        let answer = Math.ceil(Math.random() * this.cards.length); // 1'den 5'e kadar random bi sayı attırır
        console.log(answer); //consolda işlemi yazdırır 
        this.answer = this.cards[answer - 1];
        console.log(this.answer);   
    },
    methods: { 
        showCard(answer){
            //this.activeCard = answer.component; 
            if(this.selectedCard == null){
                alert("Kart Seçmediniz ! ");
            } 
            else{
                this.activeCard = answer.component;
                setTimeout(() => {
                    if(answer.id == this.selectedCard){         
                  //  alert("Tebrikler Doğru Bildiniz !");
                    this.$emit("activeComponentEvent", "app-celeb")
                }
                else{
                //    alert("Yanlış !");
                    this.$emit("activeComponentEvent", "app-lose")
                }
                },2000)
                
                
            }
        }
    }
    
}
</script>
<style scoped>
.title 
{
    text-align: center;
    font-family: sans-serif;
    color: rosybrown;
}
.title span
{
    color: mediumpurple;    
}
.title strong
{
    color: darkred;
}
.description
{
    color: gray;
    text-align: center;
}
.container, .card-container
{
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 50px;
}
.shadow
{
    box-shadow: 0px 5px 48px  #30969f !important;  /** İmportant 'hover:' in devreye girmesini engeller*/
    transition: box-shadow .5s; 
}
/******************** AÇIK KARTLARIN ANİMASYONLARI İÇİN GEREKLİ TRANSİTİON CLASS TANIMLARI*******************/
.rotate-all-enter{}
.rotate-all-enter-active{
    animation: rotate-all ease-in-out 2s forwards;
}
.rotate-all-leave{}
.rotate-all-leave-active{}
@keyframes rotate-all{
    from {
        transform: rotateY(0);
    }

    to {
        transform: rotateY(1080deg);
    }

}   
/*******************KAPALI KARTIN ANİMASYONLARI İÇİN GEREKLİ TRANSİTİON CLASS TANIMLARI*************************/
.rotate-enter{}
.rotate-enter-active{
    animation: rotate-in 1s ease-in-out forwards;
}

.rotate-leave{}
.rotate-leave-active{
    animation: rotate-out 1s ease-in-out forwards;
}
@keyframes rotate-in{
    from 
    {
        transform: rotateY(90deg);
    }
    to {
        transform: rotateY(0deg);
    }
}
@keyframes rotate-out{
    from 
    {
        transform: rotateY(0deg);
    }
    to {
        transform: rotateY(90deg);
    }
}   
</style>