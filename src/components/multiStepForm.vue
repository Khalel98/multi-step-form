<template lang="">
	<div id="multi">
	  <div class="container">
		 <article >
			<header  v-if="activeStep > 0 && activeStep < 6">
				<div class="section__header__mobile">
					<div class="section__header__mobile__content">
						<button class="section__header__mobile__btn" v-if="activeStep > 0" @click="prevStep">
								<img src="../assets/vector.png" alt="">
						</button>
						<div class="section__header__mobile__title">{{ formSteps[activeStep].titleMobile}}</div>
					</div>
				</div>
			  <div class="progress">
				 <div class="progress-step"
						:class="{'active': index === activeStep}"
						v-for="(step, index) in formSteps"
						:key="'step'+index">
				 </div>
			  </div>
			</header>
			<section :class="animation">
				<!-- Circle progressive bar -->
				<div class="circle" id="circle" v-if="activeStep > 0 && activeStep < 6">
					<div class="circle__container">
						<svg viewBox="-1 -1 34 34">
							<circle cx="16" cy="16" r="15.9155" class="progress-bar__background" />
							<circle cx="16" cy="16" r="15.9155" id="js-progress-bar" class="progress-bar__progress" :style="myStyle"/>
						</svg>
						<span class="circle__text" id="circle__text">{{ circlePage }}</span>
					</div>
				</div>	
				<div class="section__header" :class="{ removeHeaderMobile: isActive }">
					<div class="section__header__body">
						<button class="section__header__btn" @click="prevStep" v-if="activeStep >0">
							<img src="../assets/left-arrow.png" alt="">
						</button>
						<div class="section__header__content">
							<h2 class="section__header__title">{{ formSteps[activeStep].title}}</h2>
							<p class="section__header__subtitle">{{ formSteps[activeStep].subtitle}}</p>
							<p class="section__header__required section__header__subtitle"><span>*</span> міндетті түрде толтыру</p>
						</div>
					</div>
					
				</div>
			  <div class="input-fields">
				<div v-show="activeStep == 0"  class="">
					<div class="welcome__details">
						<div class="welcome__details__row">
						<div class="welcome__details__content">
							<div class="welcome__details__img">
								<img class="welcome__details__img__welcome" src="../assets/welcome.png" alt="">
								<img class="welcome__details__img__logo" src="../assets/logo.png" alt="">
							</div>
						</div>
						<h2 class="section__header__title__mobile">{{ formSteps[activeStep].title}}</h2>

						<div class="welcome__details__content">
							<ol class="welcome__details__list">
								<li class="welcome__details__item">
								Кезек санатына сай талап етілетін қажетті құжаттардың электронды нұсқадағы файлдарын жинақтаңыз 
								<br> - резюме 
								<br> - зейнетақы анықтамасы
								<br> - балалардың туу туралы куәліктері
								<br> - мүгедектігі туралы анықтама
								<br> - свидетельство о рождении 
								<br> - ЭЦҚ - Электронды цифрлық қолтаңба</li>
								<li class="welcome__details__item">Өтінім формасын дұрыс толтырып, сұралған файлдарды жүктеп жолдаңыз</li>
							</ol>
						</div>
            	</div>
					<div class="welcome__details__checkbox">
					<label for="checkbox" class="welcome__details__checkbox__body">Барлық құжаттар жинақталған және өтініш тастауға дайынмын
						<input :type=formSteps[0].fields[0].type :class="{'wrong-input': !formSteps[0].fields[0].valid}" v-model="formSteps[0].fields[0].value" :name=formSteps[0].fields[0].name name="" id="checkbox"  @click="checkhCheckbox">
						<span class="checkmark"></span>
					</label>
					</div>  
          		</div>

				</div>
				<form action="/action" methods='GET' id="authForm" @submit="submitAuthForm">
					<div v-show="activeStep == 1"  class="input-container" style="flex-direction:column; align-items:center;">
							<!--ЭЦҚ-ны жүктеңіз-->
							<div class="input-container-content">
								<label>{{formSteps[firstStep].fields[0].label}}</label>
								<input id="icp" :type=formSteps[firstStep].fields[0].type :class="{'wrong-input': !formSteps[firstStep].fields[0].valid}" v-model="formSteps[firstStep].fields[0].value" :name=formSteps[firstStep].fields[0].name style="display:none;" @change="onFileChange" />
								<label for="icp" class="inputImmitation">
									<p v-for="(file, index) in files" :key="index">
										<span v-if="file.name.length>25">{{ file.name.slice(0, 14)+ "...." + file.name.substring(file.name.length - 4) }}</span>
										<span v-if="file.name.length<25">{{ file.name }}</span>
									</p>
								</label>
							</div>
							<!--Құпия сөз-->
							<div class="input-container-content">
								<label for="">{{formSteps[firstStep].fields[1].label}}</label>
								<input :type=formSteps[firstStep].fields[1].type :class="{'wrong-input': !formSteps[firstStep].fields[1].valid}" v-model="formSteps[firstStep].fields[1].value" :name=formSteps[firstStep].fields[1].name autocomplete="off" placeholder="Құпия сөзді енгізіңіз"/>
							</div>
					</div>
					<button style="display:none;" class="" id="authFormBtn" @click="submitAuthForm">SIGN IN</button>
				</form>

				<form action="/action" id="regForm" methods='POST' @submit="submitRegForm">
					<div v-show="activeStep == 2"  class="input-container">
							<!--Тегі-->
							<div class="input-container-content">
								<label>{{formSteps[secondStep].fields[0].label}}</label>
								<input :type=formSteps[secondStep].fields[0].type :class="{'wrong-input': !formSteps[secondStep].fields[0].valid}" v-model="formSteps[secondStep].fields[0].value" :name=formSteps[secondStep].fields[0].name @keypress='onlyCyrillic(event)' autocomplete="off"  placeholder="Енгізіңіз"/>
							</div>
							<!--Аты-->
							<div class="input-container-content">
								<label for="">{{formSteps[secondStep].fields[1].label}}</label>
								<input :type=formSteps[secondStep].fields[1].type :class="{'wrong-input': !formSteps[secondStep].fields[1].valid}" v-model="formSteps[secondStep].fields[1].value" :name=formSteps[secondStep].fields[1].name @keypress='onlyCyrillic(event)' autocomplete="off" placeholder="Енгізіңіз"/>
							</div>
							<!--Әкесінің аты-->
							<div class="input-container-content">	
								<label for="">{{formSteps[secondStep].fields[2].label}}</label>
								<input :type=formSteps[secondStep].fields[2].type :class="{'wrong-input': !formSteps[secondStep].fields[2].valid}" v-model="formSteps[secondStep].fields[2].value" :name=formSteps[secondStep].fields[2].name @keypress='onlyCyrillic(event)' autocomplete="off" placeholder="Енгізіңіз"/>
							</div>
							<!--ЖСН-->
							<div class="input-container-content">
								<label for="">{{formSteps[secondStep].fields[3].label}}</label>
								<input :type=formSteps[secondStep].fields[3].type :class="{'wrong-input': !formSteps[secondStep].fields[3].valid}" v-model="formSteps[secondStep].fields[3].value" :name=formSteps[secondStep].fields[3].name @keypress='onlyNumber(event)' maxlength="12" autocomplete="off" placeholder="Енгізіңіз"/>
							</div>
							<!--Телефон нөмірі-->
							<div class="input-container-content">
								<label for="">{{formSteps[secondStep].fields[4].label}}</label>
								<div class="hey" style="display:flex; align-items:center">
									<span class="seven">+7</span>
									<input :type=formSteps[secondStep].fields[4].type :class="{'wrong-input': !formSteps[secondStep].fields[4].valid}" v-model="formSteps[secondStep].fields[4].value" :name=formSteps[secondStep].fields[4].name @keypress='onlyNumber(event)' maxlength="10" autocomplete="off"/>
								</div>
							</div>
							<!--Почта-->
							<div class="input-container-content">
								<label for="">{{formSteps[secondStep].fields[5].label}}</label>
								<input :type=formSteps[secondStep].fields[5].type :class="{'wrong-input': !formSteps[secondStep].fields[5].valid}" v-model="formSteps[secondStep].fields[5].value" :name=formSteps[secondStep].fields[5].name autocomplete="off" placeholder="Электронды почтаңызды енгізіңіз"/>
							</div>
							<!--Туған күні-->
							<div class="input-container-content">
								<label for="">{{formSteps[secondStep].fields[6].label}}</label>
								<input :type=formSteps[secondStep].fields[6].type :class="{'wrong-input': !formSteps[secondStep].fields[6].valid}" v-model="formSteps[secondStep].fields[6].value" :name=formSteps[secondStep].fields[6].name  autocomplete="off" min='1900-01-01' max='2023-01-01' id="birthday" />
							</div>
							<!--Жеке құжат ақпараты-->
							<div class="input-container-content"></div>
							<div class="input-container-content">
								<label for="">{{formSteps[secondStep].fields[7].label}}</label>
								<select :type=formSteps[secondStep].fields[7].type :class="{'wrong-input': !formSteps[secondStep].fields[7].valid}" v-model="formSteps[secondStep].fields[7].value" :name=formSteps[secondStep].fields[7].name >
									<option value="" selected style="display:none;">...</option>
									<option value="1">Жеке куалык</option>
								</select>	
							</div>
							<!--Құжат берген ұйым * (ҚР ІІМ, ҚР ӘМ)-->
							<div class="input-container-content">
								<label>{{formSteps[secondStep].fields[8].label}}</label>
								<input :type=formSteps[secondStep].fields[8].type :class="{'wrong-input': !formSteps[secondStep].fields[8].valid}" v-model="formSteps[secondStep].fields[8].value" :name=formSteps[secondStep].fields[8].name @keypress='onlyCyrillic(event)' autocomplete="off" placeholder="Енгізіңіз"/>
							</div>
							<!--Құжат нөмірі-->
							<div class="input-container-content">
								<label for="">{{formSteps[secondStep].fields[9].label}}</label>
								<input :type=formSteps[secondStep].fields[9].type :class="{'wrong-input': !formSteps[secondStep].fields[9].valid}" v-model="formSteps[secondStep].fields[9].value" :name=formSteps[secondStep].fields[9].name @keypress='onlyNumber(event)' maxlength="9" autocomplete="off" placeholder="Нөмірді енгізіңіз"/>
							</div>
							<!--Құжат жарамдылығы-->
							<div class="input-container-content">
								<label for="">{{formSteps[secondStep].fields[10].label}}</label>
								<input :type=formSteps[secondStep].fields[10].type :class="{'wrong-input': !formSteps[secondStep].fields[10].valid}" v-model="formSteps[secondStep].fields[10].value" :name=formSteps[secondStep].fields[10].name autocomplete="off" placeholder="Енгізіңіз" min='2023-01-01' max='2043-01-01' id="document_data"/>
							</div>

					</div>

					<div v-show="activeStep == 3" class="input-container">
							<!--Мамандық *-->
							<div class="input-container-content">
								<label for="">{{formSteps[thirdStep].fields[0].label}}</label>
								<select :type=formSteps[thirdStep].fields[0].type :class="{'wrong-input': !formSteps[thirdStep].fields[0].valid}" v-model="formSteps[thirdStep].fields[0].value" :name=formSteps[thirdStep].fields[0].name>
									<option value="" selected style="display:none;">...</option>
									<option value="1">1</option>
									<option value="2">2</option>
								</select>	
							</div>
							<!--Біліміңіз *-->
							<div class="input-container-content">
								<label for="">{{formSteps[thirdStep].fields[1].label}}</label>
								<select :type=formSteps[thirdStep].fields[1].type :class="{'wrong-input': !formSteps[thirdStep].fields[1].valid}" v-model="formSteps[thirdStep].fields[1].value" :name=formSteps[thirdStep].fields[1].name >
									<option value="1">Жоғарғы</option>
									<option value="2">Арнайы орта</option>
									<option value="3">Орта</option>

								</select>	
							</div>
							<!--Резюме *-->
							<div class="input-container-content">
								<label>{{formSteps[thirdStep].fields[2].label}}</label>
								<input id="resume" :type=formSteps[thirdStep].fields[2].type :class="{'wrong-input': !formSteps[thirdStep].fields[2].valid}" v-model="formSteps[thirdStep].fields[2].value" :name=formSteps[thirdStep].fields[2].name style="display:none;" @change="isFileEmpty"/>
								<label for="resume" class="btnImitation"></label>
							</div>

							<!--Оқу орны * -->
							<div class="input-container-content">
								<label>{{formSteps[thirdStep].fields[3].label}}</label>
								<input :type=formSteps[thirdStep].fields[3].type :class="{'wrong-input': !formSteps[thirdStep].fields[3].valid}" v-model="formSteps[thirdStep].fields[3].value" :name=formSteps[thirdStep].fields[3].name autocomplete="off" placeholder="Енгізіңіз"/>
							</div>

							<!--Дипломыңызды жүктеңіз *-->
							<div class="input-container-content">
								<label class="deleteRequiredMark">{{formSteps[thirdStep].fields[4].label}}</label>
								<input id="diplom" :type=formSteps[thirdStep].fields[4].type :class="{'wrong-input': !formSteps[thirdStep].fields[4].valid}" v-model="formSteps[thirdStep].fields[4].value" :name=formSteps[thirdStep].fields[4].name style="display:none;" @change="isFileEmpty"/>
								<label for="diplom" class="btnImitation"></label>
							</div>

							<!--Оқу бітірген жылы -->
							<div class="input-container-content">
								<label class="deleteRequiredMark">{{formSteps[thirdStep].fields[5].label}}</label>
								<input :type=formSteps[thirdStep].fields[5].type :class="{'wrong-input': !formSteps[thirdStep].fields[5].valid}" v-model="formSteps[thirdStep].fields[5].value" :name=formSteps[thirdStep].fields[5].name @keypress='onlyNumber(event)' maxlength='4' autocomplete="off" placeholder="Енгізіңіз"/>
							</div>
					</div>	
					
					<div v-show="activeStep == 4" class="input-container">
							<!--Отбасылық жағдай * *-->
							<div class="input-container-content">
								<label for="">{{formSteps[fouthStep].fields[0].label}}</label>
								<select :type=formSteps[fouthStep].fields[0].type :class="{'wrong-input': !formSteps[fouthStep].fields[0].valid}" v-model="formSteps[fouthStep].fields[0].value" :name=formSteps[fouthStep].fields[0].name>
									<option value="" selected style="display:none;">...</option>
									<option value="1">Бойдақ</option>
									<option value="2">Отбасылы</option>
									<option value="3">Ажырасқан</option>
									<option value="4">Жесір</option>
								</select>	
							</div>

							<!--Тұрғылықты мекенжай *  -->
							<div class="input-container-content">
								<label>{{formSteps[fouthStep].fields[1].label}}</label>
								<input :type=formSteps[fouthStep].fields[1].type :class="{'wrong-input': !formSteps[fouthStep].fields[1].valid}" v-model="formSteps[fouthStep].fields[1].value" :name=formSteps[fouthStep].fields[1].name autocomplete="off" placeholder="Енгізіңіз"/>
							</div>

							<!--Балалар саны -->
							<div class="input-container-content">
								<label class="deleteRequiredMark">{{formSteps[fouthStep].fields[2].label}}</label>
								<input :type=formSteps[fouthStep].fields[2].type :class="{'wrong-input': !formSteps[fouthStep].fields[2].valid}" v-model="formSteps[fouthStep].fields[2].value" :name=formSteps[fouthStep].fields[2].name @keypress='onlyNumber(event)' maxlength='2' autocomplete="off" placeholder="Енгізіңіз"/>
							</div>

							
							<!--Тіркелген мекенжай *  -->
							<div class="input-container-content">
								<label>{{formSteps[fouthStep].fields[3].label}}</label>
								<input :type=formSteps[fouthStep].fields[3].type :class="{'wrong-input': !formSteps[fouthStep].fields[3].valid}" v-model="formSteps[fouthStep].fields[3].value" :name=formSteps[fouthStep].fields[3].name autocomplete="off" placeholder="Енгізіңіз"/>
							</div>
					</div>
					<div v-show="activeStep == 5" class="input-container">
							<div class="input-container-content">
									<label for="">{{formSteps[fifthStep].fields[0].label}}</label>
									<select :type=formSteps[fifthStep].fields[0].type :class="{'wrong-input': !formSteps[fifthStep].fields[0].valid}" v-model="formSteps[fifthStep].fields[0].value" :name=formSteps[fifthStep].fields[0].name >
										<option value="" selected style="display:none;">...</option>
										<option value="2">Табыс көзі бар отбасы</option>
										<option value="3">Аз қамтылған</option>
										<option value="4">Мүгедек бар отбасы</option>
										<option value="5">Жетім</option>
										<option value="6">Мүгедек немесе пробацияда</option>
									</select>	
							</div>
							<!--Өлім туралы құжат(жетімдер мен асыраушылар)-->
							<div class="input-container-content">
								<label class="deleteRequiredMark">{{formSteps[fifthStep].fields[1].label}}</label>
								<input id="death_certificate" :type=formSteps[fifthStep].fields[1].type :class="{'wrong-input': !formSteps[fifthStep].fields[1].valid}" v-model="formSteps[fifthStep].fields[1].value" :name=formSteps[fifthStep].fields[1].name style="display:none;" @change="isFileEmpty"/>
								<label for="death_certificate" class="btnImitation"></label>
							</div>

							<!--Мүгедектігі жайлы құжат-->
							<div class="input-container-content">
								<label class="deleteRequiredMark">{{formSteps[fifthStep].fields[2].label}}</label>
								<input id="certificate_of_disability" :type=formSteps[fifthStep].fields[2].type :class="{'wrong-input': !formSteps[fifthStep].fields[2].valid}" v-model="formSteps[fifthStep].fields[2].value" :name=formSteps[fifthStep].fields[2].name style="display:none;" @change="isFileEmpty"/>
								<label for="certificate_of_disability" class="btnImitation"></label>
							</div>
							<!--Пробациялық бақылау туралы-->
							<div class="input-container-content">
								<label class="deleteRequiredMark">{{formSteps[fifthStep].fields[3].label}}</label>
								<input id="probation_certificate" :type=formSteps[fifthStep].fields[3].type :class="{'wrong-input': !formSteps[fifthStep].fields[3].valid}" v-model="formSteps[fifthStep].fields[3].value" :name=formSteps[fifthStep].fields[3].name style="display:none;" @change="isFileEmpty"/>
								<label for="probation_certificate" class="btnImitation"></label>
							</div>
							<!--Зейнеткерлік түсім құжаты-->
							<div class="input-container-content">
								<label class="deleteRequiredMark">{{formSteps[fifthStep].fields[4].label}}</label>
								<input id="pension_application" :type=formSteps[fifthStep].fields[4].type :class="{'wrong-input': !formSteps[fifthStep].fields[4].valid}" v-model="formSteps[fifthStep].fields[4].value" :name=formSteps[fifthStep].fields[4].name style="display:none;" @change="isFileEmpty"/>
								<label for="pension_application" class="btnImitation"></label>
							</div>
							<!--Сот үкімі-->
							<div class="input-container-content">
								<label class="deleteRequiredMark">{{formSteps[fifthStep].fields[5].label}}</label>
								<input id="verdict_court" :type=formSteps[fifthStep].fields[5].type :class="{'wrong-input': !formSteps[fifthStep].fields[5].valid}" v-model="formSteps[fifthStep].fields[5].value" :name=formSteps[fifthStep].fields[5].name style="display:none;" @change="isFileEmpty"/>
								<label for="verdict_court" class="btnImitation"></label>
							</div>
					</div>
					<button style="display:none;" class="" id="regFormBtn" @click="submitRegForm">SIGN IN</button>
				</form>
				<div v-show="activeStep == 6">
					<div class="welcome__details">
                <div class="welcome__details__row">
                  <div class="welcome__details__content" style="width: 200px;">
                    <div class="welcome__details__img">
                      <img class="welcome__details__img__welcome" src="../assets/checkmark.png" alt="" style="width: 200px;">
				  				<img class="welcome__details__img__logo" src="../assets/checkmark-mini.png" alt=""  style="width: 80px !important;">
                    </div>
                  </div>
						<h2 class="section__header__title__mobile">{{ formSteps[activeStep].title}}</h2>

                  <div class="welcome__details__content">
                    <ol class="welcome__details__list">
                      <li class="welcome__details__item">
                        Кезекке тұру өтінішіңізді Жаңаөзен қаласының жұмыспен қамту орталығы 15 тәулік ішінде тексеріп шешім қабылдайды
                      </li>
                      <li class="welcome__details__item">Шешімнің оң нәтижесі болған жағдайда кезекке автоматты түрде тіркеледі.</li>
                      <li class="welcome__details__item">Егер жіберілген өтініште қателіктер болса, почта арқылы хабарлама жіберіледі.</li>
                    </ol>
                  </div>
                </div>
      
              </div>
				</div>
				 <!--<div class="input-container"
						v-for="(field, index) in formSteps[activeStep].fields" :key="'field'+index">
					<input :type=field.type :class="{'wrong-input': !field.valid}" v-model="field.value" required />
					<label class="input-label">{{ field.label }}</label>
				 </div>-->
			  </div>
			  <div class="actions">
				 <button class='actions__btn firstStepBtn' :class="{ allowBtn: allowBtn }" v-if="activeStep === 0" @click="checkFields">Жалғастыру</button>
				 <!--submitAuthForm-->
				 <button class='actions__btn secondStepBtn' v-if="activeStep === 1"  @click="tempAuthMethod" >Тіркелу</button>
				 <button class='actions__btn' v-if="activeStep > 1 && activeStep < 5" @click="checkFields">Жалғастыру</button>
				 <!--submitRegForm-->
				 <button class='actions__btn' v-if="activeStep === 5" @click="tempRegMethod">Жіберу</button>
				 <button class='returnBtn' v-if="activeStep === 6" @click="reloadPage()">Бастапқы бетке өту</button>
			  </div>
			</section>
		 </article>
	  </div>
	</div>
	</template>
	<script>

	export default { 
	  el: '#multi',
	  data() {
		return{
			myStyle:{
				strokeDashoffset:100
			},
			files: [{name:'Файлды жүктеу'}],
			circlePage: 0,
			activeStep: 0,
			firstStep:1,
			secondStep:2,
			thirdStep:3,
			fouthStep:4,
			fifthStep:5,
			isActive: false,
			allowBtn:false,
			animation: 'animate-in',
			formSteps: [
				{
				title: "Кезекке тұру үшін ақпарат",
				fields: [
					{ value: '', valid: true, type: 'checkbox', name:'order',required: true, pattern: /.+/},
				]
				},
				{
				title: "ЭЦҚ-мен тіркелу",
				subtitle: "Жеке тұлғаларға арналған электронды-цифрлық қолтаң	бамен (ЭЦП) жүйеге кіріңіз",
				titleMobile:'ЭЦҚ-мен тіркелу',
				fields: [
					{ label: "ЭЦҚ-ны жүктеңіз", value: '', valid: true, type: 'file', name:'icp',required: true, pattern: /.+/},
					{ label: "Құпия сөз", value: '', valid: true, type: 'password', name:'password',required: true, pattern: /.+/}
				]
				},
				{
				title: "Негізгі ақпараттарды толтыру",
				subtitle: "Жеке ақпараттарды ресми құжаттармен сәйкестендіріп толтырыңыз",
				titleMobile:'Негізгі ақпараттар',
				fields: [
					{ label: "Тегі", value: '', valid: true, type: 'text', name:'last_name',required: true, pattern: /.+/},
					{ label: "Аты", value: '', valid: true, type: 'text', name:'name',required: true, pattern: /.+/},
					{ label: "Әкесінің аты", value: '', valid: true, type: 'text', name:'patronymic',required: true, pattern: /.+/},
					{ label: "ЖСН", value: '', valid: true, type: 'text', name:'document_number',required: true, pattern: /.+/},
					{ label: "Телефон нөмірі", value: '', valid: true, type: 'text', name:'phone_number',required: true, pattern: /.+/	},
					{ label: "Почта", value: '', valid: true, type: 'email', name:'email', required: true, pattern: /^[^\s@]+@[^\s@]+\.[^\s@]+$/ },
					{ label: "Туған күні", value: '', valid: true, type: 'date', name:'birthday', required: true, pattern: /.+/ },
					{ label: "Жеке құжат ақпараты", value: '', valid: true, name:'document_type', required: true, pattern: /.+/ },
					{ label: "Құжат берген ұйым * (ҚР ІІМ, ҚР ӘМ)", value: '', valid: true, type: 'text', name:'document_issued', required: true, pattern: /.+/ },
					{ label: "Құжат нөмірі", value: '', valid: true, type: 'text', name:'document_exp', required: true, pattern: /.+/ },
					{ label: "Құжат жарамдылығы", value: '', valid: true, type: 'date', name:'document_data', required: true, pattern: /.+/ },
				]
				},
				{
				title: "Біліміңіз жайлы ақпараттарды толтыру",	
				subtitle: "Ақпараттарды толтырып, оларды растайтын құжаттарды жүктеңіз",
				titleMobile:'Біліміңіз жайлы ақпараттар',
				fields: [
					{ label: "Мамандық", value: '', valid: true, name:'positions', required: true, pattern: /.+/ },
					{ label: "Біліміңіз", value: '', valid: true, name:'education_type', required: true, pattern: /.+/ },
					{ label: "Резюме", value: '', valid: true, type: 'file', name:'resume',required: true, pattern: /.+/},
					{ label: "Оқу орны", value: '', valid: true, type: 'text', name:'education_org',required: true, pattern: /.+/},
					{ label: "Дипломыңызды жүктеңіз", value: '', valid: true, type: 'file', name:'diplom',required: false, pattern: /.+/},
					{ label: "Оқу бітірген жылы ", value: '', valid: true, type: 'text', name:'education_year_finish',required: false, pattern: /.+/},
				]
				},
				{
				title: "Отбасылық жағдайыңыз жайлы ақпаратты толтыру",
				subtitle: "Ақпараттарды толтырып, оларды растайтын құжаттарды жүктеңіз",
				titleMobile:'Отбасылық жағдай',
				fields: [
					{ label: "Отбасылық жағдай", value: '', valid: true, name:'family_status', required: true, pattern: /.+/ },
					{ label: "Тұрғылықты мекенжай", value: '', valid: true, type: 'text', name:'address',required: true, pattern: /.+/},
					{ label: "Балалар саны", value: '', valid: true, type: 'text', name:'childs',required: false, pattern: /.+/},
					{ label: "Тіркелген мекенжай", value: '', valid: true, type: 'text', name:'address_reg',required: true, pattern: /.+/},

				]
				},
				{
				title: "Санатыңыз жайлы ақпараттарды толтыру",
				subtitle: "Ақпараттарды толтырып, оларды растайтын құжаттарды жүктеңіз",
				titleMobile:'Санатыңыз жайлы ақпараттар ',
				fields: [
					{ label: "Санатыңыз", value: '', valid: true, name:'privilege_id', required: true, pattern: /.+/ },	
					{ label: "Өлім туралы құжат(жетімдер мен асыраушылар)", value: '', valid: true, type: 'file', name:'death_certificate',required: false, pattern: /.+/},
					{ label: "Мүгедектігі жайлы құжат", value: '', valid: true, type: 'file', name:'certificate_of_disability',required: false, pattern: /.+/},
					{ label: "Пробациялық бақылау туралы", value: '', valid: true, type: 'file', name:'probation_certificate',required: false, pattern: /.+/},
					{ label: "Зейнеткерлік түсім құжаты", value: '', valid: true, type: 'file', name:'pension_application',required: false, pattern: /.+/},
					{ label: "Сот үкімі", value: '', valid: true, type: 'file', name:'verdict_court',required: false, pattern: /.+/},

				]
				},				
				{
				title: "Ақпараттар сәтті жіберілді!",
				fields: [
				]
				}
			],
		}
	  },
	  methods: {
		submitAuthForm(e) {
			console.log("AuthForm Sended");
			e.preventDefault();
			var authForm = document.getElementById('authForm')
			const formData = new FormData(authForm);
			const payload = new URLSearchParams(formData);
			
			this.axios.post("http://localhost:3000/auth/",payload)
			
			// .then((result)=>{
			// 	console.warn(result)
			// })
		},
		submitRegForm(e) {
			console.log("RegForm Sended");
			e.preventDefault();
			var regForm = document.getElementById('regForm')
			const formData = new FormData(regForm);
			const payload = new URLSearchParams(formData);
			
			this.axios.post("http://localhost:3000/reg/",payload)

			// .then((result)=>{
			// 	console.warn(result)
			// })
		},
		onFileChange(e) {
			var fileName = e.target.files;
				this.files = fileName 
    	},
		isFileEmpty(e){
			if(e.target.value.length == ''){
				e.target.parentNode.classList = 'input-container-content wrongInput'
			}else{
				e.target.parentNode.classList = 'input-container-content rightInput'
			}
		},
		checkhCheckbox(){
			if(this.formSteps[this.activeStep].fields[0].value == false){
				this.allowBtn = true
			}else{
				this.allowBtn = false
			}	
		},
		progressBar(){
			var percentageComplete = (this.activeStep) * 20;
			var strokeDashOffsetValue = 100 - percentageComplete;  
			this.myStyle.strokeDashoffset = strokeDashOffsetValue
			this.circlePage = (this.activeStep) + '/' + '5'
			if(this.activeStep ===0 || this.activeStep ===6){
				this.isActive = true
			}else{
				this.isActive = false
			}	 
		},
		onlyNumber(evt) {
		var theEvent = evt || window.event;

		// Handle paste
		if (theEvent.type === 'paste') {
				key = event.clipboardData.getData('text/plain');
		} else {
		// Handle key press
				var key = theEvent.keyCode || theEvent.which;
				key = String.fromCharCode(key);
		}
		var regex = /[0-9]|\./;
		if( !regex.test(key) ) {
			theEvent.returnValue = false;
			if(theEvent.preventDefault) theEvent.preventDefault();
		}
		},
		onlyCyrillic(evt) {
		var theEvent = evt || window.event;

		// Handle paste
		if (theEvent.type === 'paste') {
				key = event.clipboardData.getData('text/plain');
		} else {
		// Handle key press
				var key = theEvent.keyCode || theEvent.which;
				key = String.fromCharCode(key);
		}
		var regex = /[^а-яәіңғүұқ’“\s]/i;
		if(regex.test(key) ) {
			theEvent.returnValue = false;
			if(theEvent.preventDefault) theEvent.preventDefault();
		}
		},
		 nextStep() {
			this.animation = 'animate-out';
			setTimeout(() => {
			  this.animation = 'animate-in';
			  this.activeStep += 1;
			  this.progressBar()
			}, 60);
		 },
		 prevStep() {
			this.animation = 'animate-out';
			setTimeout(() => {
			  this.animation = 'animate-in';
			  this.activeStep -= 1;
			  this.progressBar()
			}, 600);
		 },
		 checkFields() {
			let valid = true;
			this.formSteps[this.activeStep].fields.forEach(field => {
				if(!field.required){
					
				}else if(field.name=='order'){
					if(field.value==false){
						valid = false;
						field.valid = false;
					}else{
						field.valid = true;	
					}
				}else if(field.name=='document_number'){
					if(field.value.length<12){
						valid = false;
						field.valid = false;
					}else{
						field.valid = true;	
					}
				}else if(field.name=='phone_number'){
					if(field.value.length<10){
						valid = false;
						field.valid = false;
					}else{
						field.valid = true;	
					}
				}else if(field.name=='document_exp'){
					if(field.value.length<9){
						valid = false;
						field.valid = false;
					}else{
						field.valid = true;	
					}
				}
				else if(field.type=='file'){
					if(field.value.length==0){
						valid = false;
						field.valid = false;
					}else{
						field.valid = true;	
					}
				}
				else if(field.name=='birthday'){
					var mydate1 = document.getElementById('birthday');
					var value1 = new Date(mydate1.value),
                min1 = new Date(mydate1.min),
                max1 = new Date(mydate1.max);
					if(field.value.length==0){
						valid = false;
						field.valid = false;
					}else if (value1 < min1 || value1 > max1) {
						valid = false;
						field.valid = false;
            	}else{
						field.valid = true;	
					}
				}

				else if(field.name=='document_data'){
					var mydate2 = document.getElementById('document_data');
					var value2 = new Date(mydate2.value),
                min2 = new Date(mydate2.min),
                max2 = new Date(mydate2.max);
					if(field.value.length==0){
						valid = false;
						field.valid = false;
					}else if (value2 < min2 || value2 > max2) {
						valid = false;
						field.valid = false;
            	}else{
						field.valid = true;	
					}
				}
				else if (!field.pattern.test(field.value)) {
				 valid = false;
				 field.valid = false;
			  }
			  else {
				 field.valid = true;
			  }
			});
	
			if(valid) {
			  this.nextStep();
			}
			else {
			  this.animation = 'animate-wrong';
			  setTimeout(() => {
				 this.animation = '';
			  }, 400);
			}
			return valid
		 },
		 tempAuthMethod(){
			if(this.checkFields()){
				// console.log('return true')
				document.getElementById('authFormBtn').click()
			}else{
				// console.log('return false')
			}
		},
		 tempRegMethod(){
			if(this.checkFields()){
				// console.log('return true')
				document.getElementById('regFormBtn').click()
			}else{
				// console.log('return false')
			}
		},
		reloadPage(){
			window.location.reload();
		}
	  },
	  mounted(){
		this.progressBar();
	  }
	}
	</script>
	<style lang="scss" src="../assets/css/style.scss">
	
	</style>